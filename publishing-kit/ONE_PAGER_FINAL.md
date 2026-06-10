# AI-EDDIE / Jarvis Command Center

**Self-hosted AI operations command center on Linux** · Personal home-lab project

`GitHub:` `<github-repo-url>`  ·  `LinkedIn:` `<linkedin-url>`  ·  `Contact:` `<email>`

`Screenshot:` `<docs/screenshots/portal-dashboard.png>` _(redacted)_

---

### Summary

A voice-driven assistant running on a single Linux machine: wake-word voice
control, a browser status Portal, a text-to-speech service, and a command-only
Telegram bot. A "live brief" system speaks short news/sports summaries from a
curated set of trusted sources and posts the supporting links to the Portal.
Built for reliability and security — every change is backed up, validated, and
route-tested before shipping.

---

### Tech stack

Linux (WSL2) · Bash · Python 3 · systemd user services · Kokoro TTS ·
PulseAudio · React + Vite + TypeScript · lightweight HTTP API · Telegram bot ·
JSON data contracts · Git

---

### Main features

- Voice live briefs across six categories, biased toward curated trusted sources.
- **Fast extractive narration by default**; LLM narration opt-in.
- Browser Portal with a live result card and a playback-aligned speaking aura.
- **Cache-bound follow-ups** ("tell me more," "make it shorter," "what sources
  did you use") — no re-searching.
- Scoped World Cup Watch Guide for schedule/channel questions.
- Command-only Telegram remote status.

---

### Security wins

- Disabled an open-ended remote "free-agent" gateway (remote-execution risk).
- Telegram **command-only / allowlisted**; user text never reaches a shell.
- **No secrets** in logs, output, the Portal, or docs.
- **Trusted-source-only** briefs that block instead of querying the open web.
- Narrow route triggers + specific-before-broad ordering to prevent hijacks.

---

### Troubleshooting wins

- **Latency:** profiled each stage, isolated a slow narration call, cut
  time-to-first-word from **~45s to ~5–7s** by changing the default path.
- **UI/audio sync:** moved the "speaking" signal to the real playback boundary.
- **Route hijack:** tightened a feature trigger; verified with route simulation
  that no existing route regressed.

---

### Top resume bullets

- Built and maintained a self-hosted Linux AI voice command center (Bash,
  Python, systemd) with a browser Portal, TTS, and a command-only Telegram bot.
- Reduced spoken-brief time-to-first-word from ~45s to ~5–7s via stage-timing
  diagnosis and a fast-default / opt-in-LLM redesign.
- Hardened the remote surface (allowlisted, command-only) and enforced
  backup → validate → document on every change.

---

_Home-lab project; not enterprise production. Metrics limited to locally
observed measurements. No credentials or private details included._

<!--
PDF export tips (pick one):
  • Pandoc:      pandoc ONE_PAGER_FINAL.md -o ONE_PAGER_FINAL.pdf
  • VS Code:     "Markdown PDF" extension → Export (pdf)
  • Browser:     open the rendered Markdown, Print → Save as PDF
  Fill the <...> placeholders (GitHub, LinkedIn, Contact, Screenshot) before export.
-->
