# Screenshot Checklist — AI-EDDIE / Jarvis Command Center

What to capture for the portfolio, and **how to redact** before sharing. Save
images under `docs/screenshots/` using the suggested filenames.

> ⚠️ **Global redaction rules — read before capturing anything:**
> - Never show **Telegram bot tokens**, bot usernames tied to your token, or
>   **chat/user IDs**.
> - Never show **API keys, OAuth screens, `.env` files, service-account JSON, or
>   private host keys**.
> - Blur **private hostnames, internal IPs/ports, and any private URLs**.
> - Hide your **full real name / personal email / phone number** if you don't
>   want them public.
> - Avoid showing **full sensitive script contents** — prefer output, not source.
> - Check the **terminal title bar, tab names, and shell prompt** (they often
>   leak `user@host` and full paths).
> - Crop or blur **browser bookmarks, other tabs, and notifications**.

---

## 1. Portal home / dashboard
- **File:** `docs/screenshots/portal-dashboard.png`
- **Capture:** the Portal HUD in its idle/ambient state — aura, core, status.
- **Redact:** any private URL in the address bar (use `localhost`/blurred);
  other browser tabs; notifications.

## 2. Live brief card
- **File:** `docs/screenshots/live-brief-card.png`
- **Capture:** the "live web intel" action card populated after a brief (e.g.
  cybersecurity), showing the result list.
- **Redact:** nothing sensitive should appear, but blur the address bar if it
  shows a private URL. Result titles/links from public sources are fine.

## 3. Telegram bot safe commands
- **File:** `docs/screenshots/telegram-commands.png`
- **Capture:** the command menu / a status reply demonstrating the **command-
  only** surface.
- **Redact (important):** the **bot handle if it's tied to your token**, the
  **chat/user ID**, your **profile name/photo**, and any group members. Consider
  cropping to just the command bubbles. Never show `/settoken`-style output or
  anything echoing a token.

## 4. Terminal validation output
- **File:** `docs/screenshots/validation-output.png`
- **Capture:** a clean validation run — `bash -n ... OK`, `py_compile ... OK`,
  `ACTION CARD JSON VALID`, route-simulation `[OK]` lines.
- **Redact:** the **shell prompt** (`user@host:/full/path`) — either use a
  generic prompt or blur it. Hide absolute home paths if they contain your real
  username and you'd rather not show it.

## 5. Git log / clean status
- **File:** `docs/screenshots/git-clean-status.png`
- **Capture:** `git log --oneline -5` with descriptive commits and
  `git status -sb` showing a clean, in-sync tree.
- **Redact:** the **remote URL** if it's a private repo (blur the
  `git@.../...` line), and the prompt/path as above. Commit author email if you
  don't want it public.

## 6. World Cup Watch Guide result
- **File:** `docs/screenshots/world-cup-watch.png`
- **Capture:** terminal output or Portal card from a World Cup query — show the
  intent line, mode, and the spoken answer (including the honest "channel not
  confirmed" message if that's what came back).
- **Redact:** prompt/path; private URL in the Portal address bar.

## 7. Cache follow-up result
- **File:** `docs/screenshots/cache-followup.png`
- **Capture:** a follow-up like "what sources did you use" or "make it shorter,"
  ideally next to evidence the Portal card timestamp **did not change** (proving
  it's cache-bound).
- **Redact:** prompt/path. The cached source names are public publishers and are
  fine to show.

---

## Optional / nice-to-have
- **Architecture diagram render** (`docs/screenshots/architecture.png`) — a clean
  version of the ASCII diagrams from `ARCHITECTURE.md`.
- **Aura before/after** — two frames showing the speaking aura aligned to audio
  (harder to capture; optional).

---

## Quick redaction methods
- **Blur/black-box:** any OS screenshot annotator, GIMP, or an image editor.
- **Terminal prompt:** temporarily set a generic prompt (e.g. `PS1='$ '`) before
  capturing, so no `user@host`/path leaks.
- **Browser:** use a clean window with no extra tabs/bookmarks; point at
  `localhost` where possible.
- **Double-check:** zoom to 100% and scan edges/title bars before exporting — the
  leaks are usually at the margins.

> When in doubt, **leave it out**. A slightly less detailed screenshot is always
> better than one that leaks a token, ID, or private URL.
