# Resume Bullets — AI-EDDIE / Jarvis Command Center

Honest, job-relevant bullets from a personal home-lab project. No fake metrics;
the only quantified claim is the locally observed latency improvement
(~45s → ~5–7s time-to-first-word on the live brief path).

> Suggested project label for a resume:
> **AI-EDDIE / Jarvis Command Center — Personal Linux Home-Lab Project**

---

## 10 strong project bullets

1. Built and maintained a self-hosted, Linux-based AI voice command center
   (Bash, Python, systemd user services) integrating a wake-word voice loop, a
   browser status Portal, a text-to-speech service, and a Telegram bot.
2. Diagnosed a 35–55 second spoken-response delay by adding stage-level timing
   instrumentation, isolating an LLM narration call as the bottleneck, and
   reducing time-to-first-word from ~45s to ~5–7s by making a fast extractive
   path the default and the slow path opt-in.
3. Redesigned the remote-control surface for safety: disabled a flexible
   "free-agent" gateway and replaced it with a command-only, allowlisted
   Telegram bot that never passes user text to a shell.
4. Implemented a trusted-source-only "live brief" system across six categories
   that biases search toward curated publishers and blocks rather than querying
   the open web when a category is unconfigured.
5. Designed a JSON cache layer enabling conversational follow-ups ("tell me
   more," "make it shorter," "what sources did you use") that answer from cache
   with no additional network calls or UI rewrites.
6. Fixed a UI/audio timing mismatch by moving the Portal "speaking" indicator to
   fire at actual audio playback instead of before synthesis, with a fallback
   that always clears state.
7. Added a scoped "World Cup Watch Guide" voice command (trusted-broadcaster
   bias, U.S.-viewing default) and tightened its trigger to prevent it from
   hijacking generic "where can I watch…" requests.
8. Established a repeatable change workflow — timestamped backups, syntax/type/
   JSON validation, and ordered-route simulation — and enforced "never ship on a
   failed check."
9. Maintained an ordered voice route table as a control plane, using
   specific-before-broad ordering and narrow triggers to prevent intent
   collisions, verified by a custom route simulator.
10. Authored clear internal documentation (running roadmap, architecture, and
    security rationale) so every change was reproducible and its trade-offs
    explicit.

---

## 5 shorter bullets (one-page resume)

- Built a self-hosted Linux AI voice assistant (Bash/Python/systemd) with a
  browser Portal, TTS, and a command-only Telegram bot.
- Cut spoken-brief time-to-first-word from ~45s to ~5–7s via stage-timing
  diagnosis and a fast-default / opt-in-LLM redesign.
- Hardened remote access by disabling free-agent control and moving to an
  allowlisted, command-only Telegram surface (no shell from user input).
- Designed a JSON cache enabling fast, consistent conversational follow-ups with
  no extra network calls.
- Enforced backup → validate → route-simulate → document on every change; kept
  trusted-source config stable across releases.

---

## Version A — Linux / Cloud Support

- Operated and maintained multiple Linux services (systemd user units) for a
  self-hosted assistant, keeping voice, Portal, TTS, and bot services healthy.
- Troubleshot a multi-second response latency by instrumenting each pipeline
  stage, identifying the slow component, and reducing time-to-first-word from
  ~45s to ~5–7s.
- Reduced security exposure on a personal host by removing an open-ended remote
  gateway and restricting remote access to allowlisted, read-oriented commands.
- Practiced safe operational change management: timestamped backups before
  edits, syntax/JSON validation, and verification of service health before
  committing.
- Kept trusted configuration (curated source lists) stable and auditable across
  changes, preventing silent drift.

---

## Version B — DevOps / Platform Engineering

- Architected a decoupled, JSON-contract-based system (voice listener, Portal,
  backend, TTS, bot) where components communicate via small documents for easy
  validation and replacement.
- Treated the voice route table as a control plane: specific-before-broad
  ordering, narrow triggers, and a custom route simulator to catch collisions
  before shipping.
- Built a validate-before-ship pipeline (`bash -n`, `py_compile`, `tsc`, JSON
  checks, route simulation) and a strict "no ship on red" policy.
- Optimized a critical path by changing the default execution mode (fast
  extractive) and making the expensive path opt-in, cutting time-to-first-word
  from ~45s to ~5–7s.
- Used git discipline (feature branches, descriptive commits, clean status,
  controlled pushes) and a running roadmap to keep changes reproducible.

---

## Version C — Technical Support / AI Operations

- Supported and improved an end-to-end AI voice workflow, resolving issues in
  latency, UI/audio synchronization, and command routing.
- Diagnosed and fixed a slow spoken-response complaint by measuring each stage
  and re-prioritizing the fast path, improving perceived responsiveness from
  ~45s to ~5–7s to first word.
- Resolved a confusing UI indicator (speaking aura firing early) by tracing it
  to the wrong event boundary and aligning it to actual audio playback.
- Implemented consistent, cache-backed follow-up answers so users get reliable,
  repeatable responses to natural questions.
- Documented troubleshooting steps, security decisions, and architecture for
  hand-off and reproducibility.

---

_All bullets describe a personal home-lab project and avoid claims of enterprise
production experience._
