# Resume Project Section — AI-EDDIE / Jarvis Command Center

Copy-paste-ready resume blocks. Honest, ATS-friendly, no fake metrics. The only
quantified claim is the locally observed latency improvement (~45s → ~5–7s
time-to-first-word).

> Suggested header line:
> **AI-EDDIE / Jarvis Command Center** — Personal Linux Home-Lab Project ·
> _Bash, Python, systemd, React/TypeScript, Git_ · `<github-repo-url>`

---

## Polished resume project entry

**AI-EDDIE / Jarvis Command Center — Self-Hosted Linux AI Assistant (Home Lab)**
*Bash · Python · systemd · React/Vite/TypeScript · Telegram Bot · Git*

Designed, built, and maintained a self-hosted AI operations command center on
Linux, integrating wake-word voice control, a browser status Portal, a
text-to-speech service, and a command-only Telegram bot. Diagnosed and resolved a
multi-second voice-response latency (time-to-first-word reduced from ~45s to
~5–7s), hardened the remote-control surface against execution risk, and enforced
a backup-validate-document change workflow with route-simulation testing.

---

## 3-bullet version

- Built and maintained a self-hosted Linux AI voice command center (Bash,
  Python, systemd) with a React/TypeScript Portal, TTS, and a command-only
  Telegram bot.
- Reduced spoken-brief time-to-first-word from ~45s to ~5–7s by adding
  stage-level timing, isolating the bottleneck, and making the fast path the
  default.
- Hardened the remote surface by disabling open-ended remote control and moving
  to allowlisted, command-only actions; enforced backup → validate → document on
  every change.

---

## 5-bullet version

- Built and maintained a self-hosted Linux AI voice command center (Bash,
  Python, systemd user services) integrating voice, a browser Portal, TTS, and a
  command-only Telegram bot.
- Diagnosed a 35–55s spoken-response delay using stage-level timing
  instrumentation, isolated a slow narration call, and cut time-to-first-word
  from ~45s to ~5–7s by re-prioritizing the fast execution path.
- Redesigned remote access for safety: disabled a flexible "free-agent" gateway
  and replaced it with an allowlisted, command-only interface that never passes
  user input to a shell.
- Implemented a JSON cache layer enabling consistent conversational follow-ups
  with no additional network calls, and aligned a UI state signal to actual
  audio playback.
- Established a repeatable change workflow (timestamped backups, syntax/type/JSON
  validation, ordered-route simulation) with a strict "no ship on a failed
  check" policy.

---

## Cloud Support version

**AI-EDDIE / Jarvis Command Center — Self-Hosted Linux Services (Home Lab)**

- Operated and maintained multiple Linux services (systemd user units) for a
  self-hosted assistant, keeping voice, Portal, TTS, and bot components healthy
  and verifying service status before changes.
- Troubleshot a multi-second response latency by instrumenting each pipeline
  stage, identifying the slow component, and reducing time-to-first-word from
  ~45s to ~5–7s.
- Reduced security exposure by removing an open-ended remote gateway and
  restricting remote access to allowlisted, read-oriented commands.
- Practiced safe operational change management: timestamped backups, JSON/syntax
  validation, and health verification before committing.

---

## Linux Admin version

**AI-EDDIE / Jarvis Command Center — Linux Systems Project (Home Lab)**

- Administered a Linux host running multiple long-lived services via systemd user
  units; managed audio (PulseAudio), background services, and inter-process
  JSON contracts.
- Modified and validated production-style Bash and Python scripts safely using
  `bash -n`, `python3 -m py_compile`, and JSON validation, always preceded by
  timestamped backups.
- Diagnosed and fixed a latency regression and a UI/audio synchronization issue
  by tracing each to its true boundary in the pipeline.
- Maintained a curated, auditable trusted-source configuration and prevented
  silent drift across changes.

---

## Technical Support version

**AI-EDDIE / Jarvis Command Center — End-to-End AI Workflow (Home Lab)**

- Supported and improved an end-to-end AI voice workflow, resolving issues across
  latency, UI/audio sync, and command routing.
- Diagnosed a slow spoken-response complaint by measuring each stage and
  re-prioritizing the fast path, improving perceived responsiveness from ~45s to
  ~5–7s to first word.
- Resolved a confusing UI indicator by tracing it to the wrong event boundary and
  aligning it to actual audio playback.
- Documented troubleshooting steps, security decisions, and architecture for
  reproducibility and hand-off.

---

## DevOps / Platform Engineering version

**AI-EDDIE / Jarvis Command Center — Service Architecture & Tooling (Home Lab)**

- Architected a decoupled, JSON-contract-based system (voice listener, Portal,
  backend, TTS, bot) for easy validation and component replacement.
- Treated the voice route table as a control plane (specific-before-broad
  ordering, narrow triggers) and built a custom route simulator to catch
  collisions before shipping.
- Built a validate-before-ship pipeline (`bash -n`, `py_compile`, `tsc`, JSON
  checks, route simulation) with a strict "no ship on red" policy.
- Optimized a critical path by changing the default execution mode and making
  the expensive path opt-in, cutting time-to-first-word from ~45s to ~5–7s.
- Used git discipline — feature branches, descriptive commits, clean status,
  controlled pushes — and a running roadmap for reproducibility.

---

## ATS keywords

Spread these naturally through your resume/profile (don't keyword-stuff):

`Linux`, `Bash`, `Python`, `systemd`, `services`, `PulseAudio`, `React`, `Vite`,
`TypeScript`, `REST/HTTP API`, `JSON`, `Git`, `GitHub`, `CLI`, `automation`,
`scripting`, `troubleshooting`, `root cause analysis`, `latency optimization`,
`performance tuning`, `observability`, `instrumentation`, `logging`,
`security`, `least privilege`, `allowlist`, `hardening`, `secrets management`,
`change management`, `backups`, `validation`, `testing`, `route simulation`,
`Telegram bot`, `text-to-speech (TTS)`, `voice interface`, `caching`,
`incident triage`, `documentation`, `self-hosted`, `home lab`.

> Note: keep claims honest — this is a personal/home-lab project, not enterprise
> production.
