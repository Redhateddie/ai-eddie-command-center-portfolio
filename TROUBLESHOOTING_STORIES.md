# Troubleshooting Stories (STAR) — AI-EDDIE / Jarvis Command Center

Interview-ready stories in **Situation / Task / Action / Result** format, each
ending with the skills demonstrated. All from a personal home-lab project;
sanitized of any secrets.

---

## Story 1 — Telegram remote safety redesign

**Situation.** The assistant had a flexible remote "agent" gateway over
Telegram that could interpret free-form requests and act on them. On a personal
machine holding real data, that was effectively a remote-execution surface.

**Task.** Keep useful remote access (check status from my phone) while removing
the open-ended execution risk.

**Action.** Disabled the free-agent gateway entirely. Redesigned the Telegram
bot to be **command-only**: a small allowlist of fixed, read-oriented handlers,
with user text never passed into a shell. Documented the rationale so the
trade-off was explicit, not accidental.

**Result.** Remote access went from "remote shell risk" to "remote dashboard."
The surface is now small enough to audit at a glance, and adding a capability is
a deliberate allowlist change rather than an emergent side effect.

**Skills shown:** threat modeling, least-privilege design, secure refactoring,
documenting security trade-offs.

---

## Story 2 — Live brief 35-second voice delay diagnosis

**Situation.** Asking for a spoken brief ("cybersecurity news") took roughly
35–55 seconds before any audio — unusable for a quick check.

**Task.** Find the real bottleneck and cut time-to-first-word without breaking
the trusted-source behavior, follow-ups, or the Portal.

**Action.** Instead of guessing, I added optional **stage timing** behind an
environment flag (terminal-only output, never spoken or shown in the UI). A
baseline run measured ~55 seconds and isolated the cost: web search ~2s, card
parse ~0s, and the **LLM narration step as the dominant cost** (a cloud call up
to ~40s). Since the useful data was already gathered in ~2 seconds, I made the
**fast extractive** narration the **default** and turned LLM narration into an
**opt-in** flag. Then I re-validated the cache, follow-ups, and source counts.

**Result.** The slow LLM call left the default path entirely. **Time-to-first-
word dropped from about 45 seconds to about 5–7 seconds.** Trusted source counts
(9 / 8 / 7 / 9 / 6 / 6) and follow-ups were unchanged.

**Skills shown:** systematic latency profiling, measuring before changing,
choosing the right default, regression-aware verification.

---

## Story 3 — Playback / aura timing mismatch

**Situation.** The Portal's "speaking" aura turned green before any sound and
lingered after audio ended, so the visuals didn't match what you heard. An
earlier fix — a fixed front-end delay timer — was unreliable.

**Task.** Make the speaking indicator reflect *actual* audio, not a guess, and
do it without touching synthesis or the microphone gate.

**Action.** Diagnosed that the speaking signal was emitted *before* synthesis,
and that a hardcoded delay couldn't track variable synthesis time. Moved the
start/stop signal to fire around the **actual playback** call, with a fallback
that always clears the state even if playback fails. Kept microphone gating on a
separate flag so the change couldn't affect the mic, and reverted the unreliable
timer hack on the front end.

**Result.** The aura now matches real audio start/stop. The fix addressed the
true boundary in the pipeline instead of papering over it with a timer.

**Skills shown:** root-cause analysis, understanding event timing across
services, surgical change with blast-radius awareness, reverting a failed
approach cleanly.

---

## Story 4 — Cache-based conversational follow-ups

**Situation.** After a brief, natural follow-ups ("tell me more," "make it
shorter," "what sources did you use") either weren't possible or risked
re-searching and returning *different* information than what was just spoken.

**Task.** Support follow-ups that are fast, consistent with the last answer, and
can't wander onto the open web.

**Action.** Made each brief write a **JSON cache** (category, timestamp, trusted
sources, result titles, spoken summary, mode). Built follow-up handlers that
read **only** that cache — no network calls, no Portal rewrite. Verified by
checking that the Portal card's timestamp was unchanged after a follow-up,
proving no new search occurred.

**Result.** Follow-ups are instant and always consistent with the brief they
follow. "Make it shorter" condenses the same answer rather than fetching a new
one, which is exactly the expected conversational behavior.

**Skills shown:** state/caching design, idempotent read paths, verification by
observable side effects (timestamp didn't change), API contract discipline.

---

## Story 5 — World Cup route hijack prevention

**Situation.** I added a small World Cup Watch Guide with a convenient trigger
that included broad phrases like "where can I watch…". That pattern would have
hijacked unrelated questions ("where can I watch a movie") into the World Cup
handler.

**Task.** Keep the feature useful but stop it from capturing non-World-Cup
intents.

**Action.** Tightened the trigger to fire **only when the phrase contains "world
cup."** Verified with a **route simulator** that replays phrases through the
ordered table: confirmed World Cup phrases route to the guide, generic
watch/channel phrases fall through, and every existing route (category briefs,
follow-ups, weather, "stop," etc.) still resolves to its original handler.
Backed up the route file first and re-ran the full validation suite.

**Result.** The guide now triggers precisely, with no collateral hijacking, and
no regression to any existing route.

**Skills shown:** input scoping, regression testing via simulation, careful
change management (backup → edit → validate → document).

---

## Cross-story themes for interviewers

- **Measure before you change** (latency profiling, route simulation,
  timestamp checks).
- **Fix the real boundary**, not the symptom (playback signal, default mode).
- **Security as a default**, not an afterthought (command-only remote, no
  secrets, trusted sources).
- **Reversible, validated changes** (backups, syntax/type/JSON checks,
  never ship on red).
