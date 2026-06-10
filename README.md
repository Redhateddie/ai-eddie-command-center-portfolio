# AI-EDDIE — Jarvis Command Center

A Linux-based, local AI operations command center with voice control, a live
status Portal, a command-only Telegram bot, and a trusted "live brief" news
system. Built and maintained as a home-lab project to practice real systems
work: service design, latency tuning, safe automation, and incremental,
test-before-ship delivery.

> **Disclaimer:** This is a sanitized, public-facing write-up of a personal
> home-lab project. It intentionally omits tokens, bot identifiers, private
> user IDs, internal hostnames, API keys, and full source for sensitive
> scripts. Nothing here is production enterprise software, and no real secrets
> or private endpoints are included.

---

## Summary

AI-EDDIE (internally called "Jarvis") is a self-hosted assistant that runs on a
single Linux machine. You speak to it ("Hey Jarvis, cybersecurity news"), it
pulls a short brief from a curated set of trusted sources, speaks a concise
answer, and updates a browser Portal with the supporting links. A separate
Telegram bot exposes a small, **allowlisted, command-only** surface for remote
status checks — deliberately *not* a free-form remote shell.

The project's theme is **boring reliability over flashy features**: each change
is backed up, validated, and route-tested before it ships, and security
trade-offs are made explicit rather than assumed.

---

## What problem it solves

A normal voice assistant either (a) sends everything to a cloud model and waits,
or (b) gives shallow canned answers. This project wanted something in between:

- **Fast spoken answers** from a *known, trusted* set of sources rather than the
  open web or a slow model round-trip.
- **A glanceable Portal** so the supporting links are always available without
  re-asking.
- **Safe remote access** from a phone without turning the assistant into a
  remote code-execution risk.
- **Predictable behavior** — the same phrase routes to the same action every
  time, and "stop" always interrupts.

---

## Architecture overview

```
   Voice ─▶ Wake word ─▶ Listener ─▶ Route table ─▶ Action script
                                                       │
                          ┌────────────────────────────┼───────────────┐
                          ▼                             ▼               ▼
                    Trusted live brief           TTS service       Portal card
                    (curated sources)            (Kokoro)          (browser HUD)
                          │                             │
                          ▼                             ▼
                     Cache (JSON)                 Audio playback
                          │
                          ▼
                  Cache-bound follow-ups
                  (more / shorter / sources / repeat)

   Phone ─▶ Telegram bot ─▶ Allowlisted command handlers (read-only status)
```

See [ARCHITECTURE.md](ARCHITECTURE.md) for component-level detail and data-flow
diagrams.

---

## Key features

- **Voice-driven live briefs** for fixed categories (global news, global sports,
  soccer, Premier League, Champions League, cybersecurity), each biased toward a
  curated list of trusted source names.
- **Fast extractive mode by default** — the assistant speaks a short headline
  brief immediately; slower LLM narration is opt-in via an environment flag.
- **Live status Portal** — a browser HUD that shows the latest "live web intel"
  card and a speaking-state aura that reflects when audio is actually playing.
- **Cache-bound conversational follow-ups** — "tell me more," "make it shorter,"
  "what sources did you use," "repeat" — answered from the last brief's cache,
  never re-searching the web.
- **World Cup Watch Guide** — a small, scoped command for schedule/channel
  questions, biased toward known broadcasters and defaulting to U.S. viewing.
- **Command-only Telegram bot** — remote status via a fixed allowlist of
  commands; no arbitrary shell execution.

---

## Security decisions

- **No free-agent remote control.** A general "do anything remotely" gateway was
  deliberately kept disabled; remote access is limited to allowlisted,
  read-oriented commands.
- **Command-only Telegram surface.** All bot actions map to fixed command
  handlers — user text is never passed to a shell.
- **No secrets in docs or output.** Tokens, keys, and IDs are never printed,
  logged, or echoed back into spoken/Portal output.
- **Trusted-source-only briefs.** If a category has no configured trusted
  sources, it blocks rather than searching the open web.
- **Backups before edits, validation before ship.** Every script change is
  preceded by a timestamped backup and followed by syntax/route validation.

See [SECURITY.md](SECURITY.md) for the full rationale.

---

## Performance improvements

- **Live brief time-to-first-word reduced from roughly 45 seconds to about
  5–7 seconds** by making fast extractive narration the default and turning the
  slower LLM narration into an opt-in path. (Observed locally with stage timing;
  see [CASE_STUDY.md](CASE_STUDY.md).)
- **Playback-aligned speaking signal** — the Portal's "speaking" aura now fires
  at actual audio playback instead of before synthesis, removing a visible
  timing mismatch.

---

## Technologies used

- **OS / runtime:** Linux (WSL2), Bash, Python 3, systemd user services
- **Voice:** wake-word listener, speech-to-text, Kokoro text-to-speech, PulseAudio
- **Frontend:** React + Vite Portal HUD (canvas-based visualization), TypeScript
- **Backend:** lightweight HTTP API for speaking-state events and the action card
- **Messaging:** Telegram bot (command-only)
- **Data:** JSON action card + JSON caches for follow-ups
- **Tooling:** Git, `grep`/`sed` route handling, `py_compile`/`bash -n`/`tsc`
  validation

---

## Screenshots

> _Placeholders — see [SCREENSHOT_CHECKLIST.md](SCREENSHOT_CHECKLIST.md) for what
> to capture and how to redact._

- `docs/screenshots/portal-dashboard.png` — Portal home / dashboard
- `docs/screenshots/live-brief-card.png` — Live web intel card
- `docs/screenshots/telegram-commands.png` — Telegram command menu (redacted)
- `docs/screenshots/validation-output.png` — Terminal validation output
- `docs/screenshots/git-clean-status.png` — Clean git log / status
- `docs/screenshots/world-cup-watch.png` — World Cup Watch Guide result
- `docs/screenshots/cache-followup.png` — Cache-based follow-up result

---

## What I learned

- How to **diagnose latency by stage** instead of guessing — measuring each step
  (search, parse, narration, audio) before changing anything.
- That **the right default matters more than the fanciest feature** — making the
  fast path the default and the slow path opt-in fixed the experience.
- How **UI signals drift from reality** — the aura looked wrong because it was
  triggered at the wrong point in the pipeline, not because of a rendering bug.
- That **scoping a trigger narrowly** ("only when the phrase contains 'world
  cup'") prevents surprising, broad route hijacks.
- The discipline of **backup → change → validate → route-test → document** as a
  repeatable, low-regret workflow.

---

## Employer value

This project demonstrates hands-on, day-to-day **Linux operations and support
engineering**: standing up and maintaining services, reading and modifying
existing scripts safely, diagnosing performance problems methodically, making
explicit security trade-offs, and documenting changes so they're reproducible.
It's the kind of careful, incremental work that keeps real systems healthy.

---

## Status

Active home-lab project. Core voice, Portal, Telegram, live brief, follow-up,
and World Cup features are implemented and locally tested; some items remain in
"browser/voice test pending" verification stages, which is reflected honestly in
the internal roadmap.

---

_This README is part of a sanitized portfolio package. It describes the design
and decisions of a personal project and deliberately excludes any sensitive
configuration._
