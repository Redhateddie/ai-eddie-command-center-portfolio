# AI-EDDIE / Jarvis Command Center — One-Pager

**Personal Linux home-lab project** · Voice-driven AI operations command center
· Sanitized portfolio summary (no secrets)

---

## Project summary

A self-hosted assistant running on a single Linux machine: wake-word voice
control, a browser status Portal, a text-to-speech service, a command-only
Telegram bot, and a "live brief" system that speaks short news/sports summaries
from a curated set of trusted sources. Designed around reliability and security
— every change is backed up, validated, and route-tested before it ships.

---

## Tech stack

- **OS/runtime:** Linux (WSL2), Bash, Python 3, systemd user services
- **Voice/audio:** wake-word listener, speech-to-text, Kokoro TTS, PulseAudio
- **Frontend:** React + Vite Portal HUD, TypeScript (canvas visualization)
- **Backend:** lightweight local HTTP API (speaking-state events, action card)
- **Messaging:** Telegram bot (command-only, allowlisted)
- **Data:** JSON action card + JSON caches
- **Tooling:** Git, route simulation, `bash -n` / `py_compile` / `tsc` / JSON
  validation

---

## Main features

- Voice live briefs across six fixed categories, biased toward curated trusted
  sources (counts held steady at 9 / 8 / 7 / 9 / 6 / 6).
- **Fast extractive narration by default**; slower LLM narration is opt-in.
- Browser Portal showing the latest result card and a playback-aligned speaking
  aura.
- **Cache-bound follow-ups** — "tell me more," "make it shorter," "what sources
  did you use," "repeat" — with no re-searching.
- **World Cup Watch Guide** for schedule/channel questions, scoped to fire only
  on phrases containing "world cup."
- Command-only Telegram remote status.

---

## Security wins

- Disabled an open-ended remote "free-agent" gateway (remote-execution risk).
- Telegram is **command-only / allowlisted**; user text never reaches a shell.
- **No secrets** in logs, spoken output, the Portal, or docs; debug timing is
  terminal-only.
- **Trusted-source-only** briefs that block instead of querying the open web when
  unconfigured.
- Narrow route triggers + specific-before-broad ordering to prevent hijacks.

---

## Troubleshooting wins

- **Latency:** profiled each stage, isolated a slow narration call, and cut
  time-to-first-word from **~45s to ~5–7s** by changing the default path.
- **UI/audio sync:** moved the "speaking" signal to the real playback boundary,
  fixing an aura that fired before audio.
- **Route hijack:** tightened the World Cup trigger and verified with a route
  simulator that no existing route regressed.

---

## Resume bullets (top 3)

- Built and maintained a self-hosted Linux AI voice command center (Bash,
  Python, systemd) with a browser Portal, TTS, and a command-only Telegram bot.
- Reduced spoken-brief time-to-first-word from ~45s to ~5–7s by adding stage
  timing, isolating the bottleneck, and making the fast path the default.
- Hardened the remote surface by disabling free-agent control and moving to an
  allowlisted, command-only interface; enforced backup → validate → document on
  every change.

---

_Home-lab project; not enterprise production. Metrics are limited to locally
observed measurements. No credentials or private details included._
