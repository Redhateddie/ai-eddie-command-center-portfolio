# LinkedIn Featured Setup — AI-EDDIE / Jarvis Command Center

How to add this project to your LinkedIn **Featured** section, plus ready-to-use
copy. All text below is sanitized — no secrets, IDs, or private URLs.

> Fill the `<...>` placeholders with your real links once the GitHub repo is
> public.

---

## Step-by-step: add the GitHub repo to LinkedIn Featured

1. Make sure the GitHub repo is **public** and the README renders cleanly
   (`https://github.com/Redhateddie/ai-eddie-command-center-portfolio`).
2. Go to your LinkedIn profile (**View profile**).
3. Click **Add profile section** (or the **+** near the top card).
4. Choose **Recommended → Add featured**.
5. In the Featured panel, click the **+** and select **Add a link**.
6. Paste the GitHub repo URL. LinkedIn will auto-pull a title/preview; wait for
   the preview to load.
7. Edit the **title** and **description** of the featured item using the copy
   below (LinkedIn lets you override what it auto-pulled).
8. (Optional) Click the image area to **upload a redacted screenshot** as the
   thumbnail — only after redaction (see reminder at the bottom).
9. Click **Save**. Reorder Featured items so this one sits first if you want it
   prominent.
10. Repeat **Add a link** if you also want to feature a LinkedIn post about the
    project (caption below) or the one-pager PDF.

---

## Short project title

> **AI-EDDIE — Local AI Operations Command Center (Linux)**

Alternates:
- **AI-EDDIE / Jarvis Command Center — Self-Hosted Voice AI on Linux**
- **AI-EDDIE: Voice-Driven Linux Command Center (Home Lab)**

---

## Short project description (1–2 lines, for the preview)

> A self-hosted Linux AI command center: wake-word voice control, a browser
> status Portal, text-to-speech, and a command-only Telegram bot. Focused on
> latency, safe automation, and reproducible change management.

---

## Longer LinkedIn Featured description

> **AI-EDDIE / Jarvis Command Center** is a personal home-lab project: a
> self-hosted AI assistant running on a single Linux machine. It combines a
> wake-word voice loop, a browser status Portal (React/Vite), a Kokoro
> text-to-speech service, and a **command-only** Telegram bot, plus a "live
> brief" system that speaks short news and sports summaries from a curated set
> of trusted sources.
>
> The engineering focus was reliability and security, not flash:
> • Diagnosed a 35–55s spoken-response delay with stage-level timing and cut
>   time-to-first-word from ~45s to ~5–7s by making a fast path the default.
> • Hardened the remote surface — disabled an open-ended "free-agent" gateway
>   and restricted remote access to allowlisted, command-only actions.
> • Built cache-backed conversational follow-ups, a playback-aligned UI signal,
>   and a narrowly-scoped feature command, each verified by route simulation.
> • Enforced a backup → validate → route-test → document workflow; nothing
>   shipped on a failed check.
>
> Repo includes a full case study, architecture diagrams, a security write-up,
> STAR troubleshooting stories, and resume/interview material.
> (Sanitized home-lab project — no production/enterprise claims, no secrets.)

---

## Suggested post caption

> Just published the write-up for **AI-EDDIE**, a local AI command center I
> built and maintain on Linux in my home lab.
>
> It's a voice assistant that speaks short briefs from trusted sources, shows
> the links on a browser Portal, and can be checked remotely through a
> command-only Telegram bot. The interesting work was the engineering around it:
> profiling a slow voice response down from ~45s to ~5–7s to first word, and
> deliberately locking down the remote surface so it couldn't become a
> remote-execution risk.
>
> Full case study, architecture, and security notes in the repo 👇
> `<github-repo-url>`
>
> #Linux #Python #HomeLab #DevOps #Troubleshooting #Security

---

## ⚠️ Reminder: screenshots only after redaction

Before uploading **any** image to LinkedIn (thumbnail or post):
- No Telegram **tokens**, bot handles tied to your token, or **chat/user IDs**.
- No API keys, OAuth screens, `.env` files, service-account JSON, or host keys.
- Blur private hostnames, internal IPs/ports, private URLs, and the terminal
  `user@host:/path` prompt.
- See `../SCREENSHOT_CHECKLIST.md` for the full list. **When in doubt, leave it
  out.**
