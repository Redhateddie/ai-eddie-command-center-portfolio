# Screenshot Plan

The redacted screenshots to capture for the portfolio, where they live, and the
rules for redacting them. **Raw/unredacted captures must never be committed** —
stage them in the gitignored `raw-screenshots/` or `private-screenshots/`
folders, redact, then export the clean version into `docs/screenshots/`.

---

## Required redacted screenshots

| Filename | What it shows |
|---|---|
| `portal-dashboard.png` | Jarvis Portal main dashboard / command center |
| `live-brief-card.png` | Live brief result card — sources visible, no private data |
| `telegram-safe-commands.png` | Telegram bot command list / safe command output, **bot & user IDs redacted** |
| `terminal-validation.png` | Terminal showing clean validation, git status, or service status (harmless local paths OK; nothing sensitive) |
| `world-cup-watch-guide.png` | World Cup Watch Guide result |
| `cache-followup.png` | A follow-up example, e.g. "tell me more" or "what sources did you use" |

Status: **none captured yet** — placeholders only. Add images here as they're
redacted, then update `README.md` to switch the checklist entry to a live image.

---

## Redaction rules (read before capturing or committing)

Hide / blur / black-box all of the following before an image leaves
`raw-screenshots/`:

- **Telegram bot token**
- **Chat IDs and user IDs**
- **API keys**
- **OAuth data** (consent screens, codes, scopes)
- **Private URLs** (anything non-public; prefer `localhost`)
- **Service-account files / JSON**
- **Exact secrets** of any kind
- **Anything that looks like a credential** (long random strings, key-like values)

Allowed:
- **General local paths**, only if they're harmless (no usernames/hosts you want
  private, no secret-bearing paths).

Hard rule:
- **Do not commit raw screenshots.** Only redacted images in `docs/screenshots/`
  are ever tracked.

---

## Workflow

1. Capture into the gitignored `raw-screenshots/` (or `private-screenshots/`).
2. Redact per the rules above (blur tokens/IDs/URLs, the `user@host:/path`
   prompt, other tabs/notifications).
3. Export the clean image to `docs/screenshots/<name>.png` using the filenames
   in the table.
4. Re-run the repo secret scan.
5. Update `README.md`: change that row from a checklist item to a real
   `![alt](docs/screenshots/<name>.png)` image.
6. Commit and push the redacted image + README update.

> See `../../SCREENSHOT_CHECKLIST.md` for the detailed per-shot capture/redaction
> guidance. When in doubt, leave it out.
