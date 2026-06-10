# Case Study — AI-EDDIE / Jarvis Command Center

> Sanitized write-up of a personal home-lab project. No secrets, tokens, IDs, or
> private endpoints are included.

---

## Background

I wanted a voice assistant I actually controlled — one that ran on my own Linux
machine, answered from sources I trusted, and didn't quietly become a remote
attack surface. Off-the-shelf assistants are either cloud-heavy and slow, or
shallow and canned. So I built **AI-EDDIE** (internally "Jarvis"): a local
operations command center combining a wake-word voice loop, a browser Portal, a
text-to-speech service, a command-only Telegram bot, and a "live brief" system
that pulls short news/sports briefings from a curated set of trusted sources.

The project doubles as a deliberate practice ground for the skills that matter
in Linux/cloud support and platform work: running services, modifying scripts
safely, diagnosing latency, and making security trade-offs explicit.

---

## Problem

Three problems drove most of the work:

1. **Speed.** Early voice briefs took roughly 35–55 seconds before any audio
   came out. That's unusable for a "quick, what's happening" interaction.
2. **Trust and safety.** A remote control path (originally a flexible
   "free-agent" gateway over Telegram) was powerful but risky — effectively
   remote execution. The assistant needed remote *access* without remote
   *danger*.
3. **Believable UI feedback.** The Portal's "speaking" aura lit up at the wrong
   time, so the visual state didn't match what you actually heard.

---

## Constraints

I imposed hard rules on myself to keep the project safe and reversible:

- No secrets in logs, output, spoken text, or documentation.
- No arbitrary shell execution from Telegram — fixed command handlers only.
- Back up any script before editing it (timestamped copies kept on disk).
- Validate every change (`bash -n`, `py_compile`, `tsc`, JSON checks, route
  simulation) before shipping, and never push if validation failed.
- One feature per change; show the diff and re-test before continuing.
- Don't touch the synthesis engine, wake word, or trusted-source data unless the
  task specifically required it.

These constraints shaped *how* every milestone below was delivered, not just
*what* was delivered.

---

## Architecture

At a high level:

- A **wake-word listener** captures audio, transcribes it, and matches the text
  against an **ordered route table**.
- Matching routes invoke **action scripts** — e.g. a live brief for a fixed
  category, a cache-bound follow-up, or the World Cup Watch Guide.
- Live briefs run a **trusted-source-biased web search**, publish a **JSON
  "action card"** that the **Portal** renders, then speak a short answer via the
  **TTS service**, which plays audio through PulseAudio.
- Each brief writes a **JSON cache** so conversational **follow-ups** ("tell me
  more," "make it shorter," "what sources did you use") can answer without
  re-searching.
- A separate **Telegram bot** exposes a small, allowlisted, read-oriented command
  set for remote status.

Full diagrams are in [ARCHITECTURE.md](ARCHITECTURE.md).

---

## Major milestones

**1. Trusted live brief system.**
Replaced open-web answers with a curated model: each category (global news,
global sports, soccer, Premier League, Champions League, cybersecurity) is
biased toward a configured list of trusted source *names*. If a category has no
configured sources, it **blocks** instead of searching the open web. Source
counts settled at **9 / 8 / 7 / 9 / 6 / 6** and have stayed there.

**2. Conversational follow-ups (cache-bound).**
After each brief, the system writes a JSON cache (category, timestamp, sources,
result titles, spoken summary, mode). Follow-ups read *only* that cache and never
hit the network or rewrite the Portal card — so "make it shorter" can't
accidentally pull new, different information.

**3. Latency fix — fast extractive default.**
See troubleshooting below. The headline result: time-to-first-word dropped from
roughly 45 seconds to about 5–7 seconds by making fast extractive narration the
default and turning LLM narration into an opt-in flag.

**4. Playback-aligned aura fix.**
Moved the Portal "speaking" signal so it fires at actual audio playback instead
of before synthesis, fixing a persistent visual/audio mismatch.

**5. World Cup Watch Guide + route tightening.**
Added a small, scoped command for World Cup schedule/channel questions, biased
toward known broadcasters and defaulting to U.S. viewing. Then tightened its
voice trigger so it only fires on phrases containing "world cup" — preventing it
from hijacking generic "where can I watch…" questions.

---

## Troubleshooting examples

### The 35–55 second voice delay

**Symptom:** "Hey Jarvis, cybersecurity news" took ~35–55 seconds before audio.

**Approach:** Rather than guess, I added optional **stage timing** (gated behind
an environment variable, printed only to the terminal — never spoken or written
to the Portal). A baseline run measured ~55 seconds with the breakdown showing:

- web search: ~2 seconds
- card parse: ~0 seconds
- **LLM narration: the dominant cost (up to a ~40-second cloud call)**
- audio: the remainder

**Fix:** The useful work was already done in ~2 seconds. So I made the **fast
extractive** path (speak the top trusted-source headlines immediately) the
**default**, and turned the slower LLM narration into an **opt-in** flag. The
LLM call dropped out of the default path entirely.

**Result:** Total dropped to ~28 seconds *including the full spoken playback* of
the brief; **time-to-first-word fell from ~45 seconds to ~5–7 seconds.** Then I
verified the cache, follow-ups, and source counts were all still intact.

### The speaking aura fired too early

**Symptom:** The Portal aura turned "speaking" green before any audio, and
lingered afterward. An earlier attempt — a fixed front-end delay timer — was
unreliable because synthesis time varies per phrase.

**Diagnosis:** The signal was emitted *before* synthesis, not at playback. A
hardcoded delay can't track variable synthesis latency.

**Fix:** Moved the start/stop signal to fire around the actual audio playback
call, with a safety fallback so the state always clears. The aura now follows
real audio. (Microphone gating was kept on a separate flag so this change
couldn't accidentally break the mic.)

### A route that was too greedy

**Symptom:** The new World Cup trigger included broad patterns like "where can I
watch…", which would have hijacked unrelated questions ("where can I watch a
movie").

**Fix:** Narrowed the trigger to fire **only when the phrase contains "world
cup."** Verified with a route simulator: World Cup phrases route correctly,
generic watch/channel phrases fall through, and every pre-existing route (briefs,
follow-ups, weather, "stop," etc.) still resolves to the same handler.

---

## Security decisions

- **Disabled the free-agent remote gateway.** Flexible remote execution is a
  liability on a personal machine; the value wasn't worth the risk.
- **Made Telegram command-only.** Every remote action is a fixed handler behind
  an allowlist; user text never reaches a shell.
- **Kept secrets out of everything public** — no tokens/IDs/keys in logs,
  spoken output, the Portal, or this portfolio.
- **Trusted-source-only briefs** with explicit blocking when unconfigured, so
  the assistant never invents sources or wanders the open web.
- **Backup-and-validate workflow** so changes are reversible and verified.

Full detail in [SECURITY.md](SECURITY.md).

---

## Results

- **Latency:** live brief time-to-first-word reduced from ~45s to ~5–7s.
- **Stability:** speaking aura now matches real audio; mic gating untouched.
- **Safety:** remote surface reduced to allowlisted, read-oriented commands.
- **Predictability:** route changes verified by simulation; trusted source
  counts held steady at 9 / 8 / 7 / 9 / 6 / 6 across all the changes above.
- **Discipline:** every change backed up, validated, route-tested, and
  documented in a running roadmap before being committed.

---

## Future improvements

- Optionally surface a confirmed broadcaster/channel in the World Cup guide when
  sources clearly state one (today it honestly says when it can't confirm).
- A short spoken "pulling your brief…" pre-line (deferred — it adds a second
  blocking audio call).
- Broaden follow-ups to more categories and richer cached context.
- Add lightweight automated tests around the route table so regressions are
  caught without manual simulation.
- Container/service hardening notes and a reproducible setup script for a fresh
  machine.

---

_This case study describes a personal home-lab project. It is intentionally
sanitized and contains no credentials or private configuration._
