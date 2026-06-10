# Interview Cheat Sheet — AI-EDDIE / Jarvis Command Center

A practical, quick-glance prep sheet. Honest about scope (home lab). Pulls from
`../INTERVIEW_PITCH.md` and `../TROUBLESHOOTING_STORIES.md`.

---

## 30-second answer

"In my home lab I built AI-EDDIE — a local AI command center on Linux. It's a
voice assistant that speaks short briefs from trusted sources, shows the links
on a browser Portal, and can be checked remotely through a command-only Telegram
bot. The fun part was the engineering: I cut the spoken-response time from around
45 seconds to about 5–7 by profiling each stage, and I locked down the remote
surface so it couldn't become a remote-execution risk."

---

## 60-second answer

"AI-EDDIE is a self-hosted assistant I built and maintain on a Linux machine. It
ties together a wake-word voice loop, a browser status Portal, a text-to-speech
service, and a Telegram bot. Ask it for something like 'cybersecurity news' and
it searches a curated set of trusted sources, posts the results to the Portal,
and speaks a short summary.

Two things I'm proud of. Performance: briefs used to take 35 to 55 seconds before
any audio, so I added stage timing, found an optional narration step was the
bottleneck, and made the fast path the default — first word now lands in about
5–7 seconds. Security: I disabled an open-ended remote gateway and replaced it
with an allowlisted, command-only Telegram interface, so user text never reaches
a shell. It's a home-lab project, but it's been a realistic way to practice Linux
operations, troubleshooting, and safe change management."

---

## 2-minute technical answer

"It's a set of small, decoupled components that talk through JSON.

A wake-word listener transcribes what I say and matches it against an ordered
route table — that table is the control plane. The first matching route runs and
exits, so order matters: specific routes come before broad ones to avoid
collisions.

For a live brief, the action runs a trusted-source-biased search, publishes a
JSON 'action card' the React Portal renders, then speaks a short answer through a
persistent TTS service that plays audio via PulseAudio. Each brief also writes a
JSON cache, which powers follow-ups like 'tell me more' or 'what sources did you
use' — those read only from cache, so they're fast and consistent and never hit
the network.

On latency: the briefs were slow because they always made an LLM narration call
that could take up to ~40 seconds. I added optional, terminal-only stage timing,
confirmed search and parse were ~2 seconds while narration dominated, and flipped
the default to a fast extractive answer with LLM narration as an opt-in flag.
Time-to-first-word went from ~45s to ~5–7s.

On the UI: the Portal 'speaking' aura fired before audio because the signal was
emitted before synthesis. I moved it to the actual playback boundary, with a
fallback that always clears the state, and kept mic gating on a separate flag so
listening wasn't affected.

And on safety: the remote surface is command-only and allowlisted, there are no
secrets in logs or output, and every change goes through backup, validation, and
route simulation before it ships."

---

## STAR story summaries (one-liners + the point)

| Story | One-line summary | Skills it shows |
|---|---|---|
| **Telegram safety redesign** | Disabled a free-agent remote gateway; moved to allowlisted command-only handlers. | Threat modeling, least privilege, secure refactoring |
| **35s voice delay** | Added stage timing, found the LLM narration call was the cost, made fast extractive the default. | Latency profiling, measure-before-change, regression checks |
| **Aura/playback mismatch** | Moved the speaking signal to the real playback boundary instead of a guess timer. | Root-cause analysis, event timing, blast-radius awareness |
| **Cache follow-ups** | Briefs write a JSON cache; follow-ups read only cache (no network). | Caching/state design, idempotent reads, verify by side effects |
| **World Cup route hijack** | Tightened trigger to fire only on "world cup"; verified with route simulation. | Input scoping, regression testing, change management |

> Full STAR write-ups: `../TROUBLESHOOTING_STORIES.md`.

---

## Common questions & answers

**"Walk me through the project."** → Use the 60-second answer above.

**"Why build it yourself instead of using an off-the-shelf assistant?"**
"I wanted control and trust — fast answers from sources I curate, on a machine I
own, with a remote surface I could actually reason about. It also let me practice
real operations work end to end."

**"What's the architecture in one breath?"**
"Decoupled components over JSON: voice listener → ordered route table → action
scripts → trusted-source search → Portal card + TTS, with a JSON cache powering
follow-ups, and a command-only Telegram bot for remote status."

**"How do you know your changes didn't break anything?"**
"Backups before edits, then `bash -n` / `py_compile` / `tsc` / JSON validation,
plus a route simulator that replays phrases through the table. I also check
invariants — like the trusted-source counts and that follow-ups don't change the
Portal timestamp. Nothing ships on a failed check."

**"What's a mistake you made?"**
"My first aura fix was a hardcoded front-end delay timer. It was unreliable
because synthesis time varies. That pushed me to stop guessing and fix the real
boundary — emit the signal at actual playback. Good reminder to measure, not
assume."

---

## "What did you secure?"

"The remote surface, mainly. I disabled an open-ended 'free-agent' gateway that
was effectively remote code execution, and replaced it with an allowlisted,
command-only Telegram interface where user text never reaches a shell. I also
kept all secrets out of logs, spoken output, the Portal, and docs — even debug
timing prints only to the terminal — and made the live briefs trusted-source-only
so they block instead of wandering the open web."

---

## "What did you troubleshoot?"

"Three notable ones: a 35–55-second voice delay (instrumented each stage, found
the slow narration call, changed the default), a speaking-indicator that fired
before audio (traced to the wrong event boundary and aligned it to playback), and
a too-greedy voice route (tightened the trigger and verified with simulation that
nothing else regressed)."

---

## "What did you automate?"

"The whole voice-to-answer pipeline — a spoken request automatically runs a
trusted-source search, updates the Portal, speaks a summary, and caches the
result for follow-ups, all without manual steps. I also automated my own
safety: a repeatable backup → validate → route-simulate → document workflow so
changes are consistent and reversible, and a route simulator so I don't have to
manually reason about every phrase."

---

## "What would you improve next?"

"Surface a confirmed broadcaster in the World Cup guide when sources clearly
state one — today it honestly says when it can't confirm a channel. Add
lightweight automated tests around the route table so I'm not relying on manual
simulation. Write a reproducible setup script and service-hardening notes for a
fresh machine. And broaden the cached follow-ups to more categories."

---

## Quick facts to have ready

- **Stack:** Linux, Bash, Python, systemd, React/Vite/TypeScript, Telegram bot,
  Kokoro TTS, JSON, Git.
- **Headline metric:** time-to-first-word ~45s → ~5–7s (locally observed).
- **Trusted-source counts (stable):** 9 / 8 / 7 / 9 / 6 / 6.
- **Scope honesty:** personal home lab, not enterprise production.
