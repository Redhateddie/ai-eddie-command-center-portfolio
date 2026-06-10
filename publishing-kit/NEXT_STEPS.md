# Next Steps — Publishing Checklist

The remaining actions to take the AI-EDDIE portfolio from "ready locally" to
"live and job-search ready." Work top to bottom.

---

## 1. Push the GitHub repo

The local repo is committed on `main`; it just needs a remote + push.

**Option A — GitHub CLI** (after `sudo apt install gh -y` and `gh auth login`):
```
cd ~/portfolio/ai-eddie-command-center-portfolio
gh repo create Redhateddie/ai-eddie-command-center-portfolio \
  --public --source=. --remote=origin --push \
  --description "Sanitized portfolio case study for a Linux-based AI operations command center with voice, Telegram, trusted live briefs, and safe automation."
```

**Option B — SSH (no gh needed; reuses your working key):**
1. Create an **empty** public repo at https://github.com/new
   (owner `Redhateddie`, name `ai-eddie-command-center-portfolio`, no
   README/.gitignore/license).
2. Then:
```
cd ~/portfolio/ai-eddie-command-center-portfolio
git remote add origin git@github.com:Redhateddie/ai-eddie-command-center-portfolio.git
git push -u origin main
```

✅ Done when: the repo is public and the README renders on GitHub.

---

## 2. Add screenshots (redacted)

- Capture the shots in `../SCREENSHOT_CHECKLIST.md`.
- **Redact first:** no tokens, bot handles tied to your token, chat/user IDs,
  API keys, OAuth screens, private URLs, internal IPs, or the `user@host:/path`
  prompt.
- Save to `docs/screenshots/` using the suggested filenames.
- Commit and push the images.

✅ Done when: README screenshot section shows real (redacted) images.

---

## 3. Add the GitHub repo to LinkedIn Featured

- Follow `LINKEDIN_FEATURED_SETUP.md` step by step.
- Use the provided title, short description, and longer Featured description.
- Optionally publish the suggested post caption and feature it too.
- Only upload a screenshot thumbnail **after** redaction.

✅ Done when: the repo link appears in your LinkedIn **Featured** section.

---

## 4. Update your resume

- Paste the right block from `RESUME_PROJECT_SECTION.md` (Cloud Support / Linux
  Admin / Technical Support / DevOps) based on the role you're applying to.
- Use the 3-bullet version for a one-page resume, 5-bullet for two pages.
- Weave in a few ATS keywords naturally — don't keyword-stuff.
- Add the GitHub link to the project header.

✅ Done when: your resume has the project with a working repo link.

---

## 5. Create a PDF from ONE_PAGER_FINAL.md

- Fill the `<...>` placeholders (GitHub, LinkedIn, Contact, Screenshot) first.
- Export to PDF — pick one:
  - **Pandoc:** `pandoc ONE_PAGER_FINAL.md -o ONE_PAGER_FINAL.pdf`
  - **VS Code:** "Markdown PDF" extension → Export (pdf)
  - **Browser:** open the rendered Markdown → Print → Save as PDF
- Keep it to a single page; tidy any spacing after export.

✅ Done when: you have a clean one-page `ONE_PAGER_FINAL.pdf` to attach to
applications.

---

## 6. Practice the interview pitch

- Rehearse the 30s / 60s / 2-minute answers in `INTERVIEW_CHEAT_SHEET.md` out
  loud until they're natural (not memorized word-for-word).
- Be ready for: "what did you secure / troubleshoot / automate / improve next."
- Have the quick facts ready (stack, the ~45s → ~5–7s metric, home-lab scope).
- Always frame it honestly as a **home-lab** project.

✅ Done when: you can give the 60-second version smoothly without notes.

---

## Reminders

- This is a **sanitized** portfolio — keep it that way. No secrets, IDs, private
  URLs, or raw screenshots ever get committed or uploaded.
- Re-run the secret scan before any push that adds new files.
- Don't claim enterprise/production experience — "home lab" is both honest and
  still impressive for the roles this targets.
