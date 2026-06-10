# Security — AI-EDDIE / Jarvis Command Center

> A security-focused write-up of design decisions for a personal home-lab
> project. This document contains **no** tokens, bot identifiers, user IDs, API
> keys, OAuth data, service-account files, host keys, or private URLs — by
> design.

The guiding principle: **a personal assistant should expand what I can do, not
expand what an attacker can do.** Every convenience was weighed against the
attack surface it created.

---

## 1. Why free-agent remote control was risky

An early design had a flexible remote "agent" gateway: send it a request and it
would figure out how to act. Convenient — and effectively a path toward remote
code execution on a personal machine.

Risks:
- A leaked or guessed credential could turn into arbitrary actions.
- "Figure out how to act" is hard to bound; the blast radius is the whole host.
- It's difficult to audit what *could* happen, only what *did* happen.

**Decision:** keep that gateway **disabled**. The flexibility wasn't worth an
open-ended remote execution surface on a machine that holds personal data.

---

## 2. Why Telegram became command-only

Telegram is a great remote control surface *if* it's tightly bounded. Instead of
interpreting free-form text remotely, the bot maps a small set of supported
messages to **fixed command handlers**.

- **Allowlist, not interpreter.** Unknown input is ignored or gets a safe reply.
- **No shell from user text.** Handlers call fixed, predefined operations; user
  strings are never concatenated into a shell command.
- **Read-oriented.** The remote surface focuses on status/awareness, not
  destructive actions.

This converts Telegram from "remote shell risk" into "remote dashboard."

---

## 3. Allowlisted remote access

The model is least-privilege by default:

- Each remote capability is an explicit, named handler.
- Adding a capability is a deliberate act (extend the allowlist), not an
  emergent side effect of a flexible parser.
- The surface is small enough to reason about in one sitting.

---

## 4. No secrets in public docs or output

A standing rule across the whole project:

- Tokens, keys, IDs, and credentials are **never** printed to logs, spoken
  aloud, written to the Portal, or included in error messages.
- Diagnostic instrumentation (e.g. latency timing) prints to the **terminal
  only** and is never spoken or shown in the UI.
- This portfolio was written to the same standard — it describes design, not
  configuration. Where a real value would normally go, there is a description
  instead.

---

## 5. Safer routing

The voice route table is a control plane, so it's treated like one:

- **Specific before broad.** Category-specific routes are matched before any
  generic catch-all, so a broad pattern can't swallow a specific intent.
- **Narrow triggers.** When a new command risked being too greedy (the World Cup
  guide originally matched generic "where can I watch…"), the trigger was
  tightened to fire **only** on phrases containing "world cup."
- **Cache-bound follow-ups.** Follow-up commands answer from cache and cannot
  trigger new network calls or rewrite the Portal — limiting what a follow-up
  can do.
- **Deterministic "stop."** The interrupt path is preserved across changes.
- **Verified by simulation.** Route edits are replayed through a simulator that
  reports the first matching handler per phrase, so collisions are caught before
  shipping.

---

## 6. Trusted sources

Live briefs are **trusted-source-only**:

- Each category is biased toward a configured list of trusted source *names*.
- If a category has no configured sources, the brief **blocks** — it does not
  fall back to the open web and does not invent sources.
- Source counts are tracked and held steady (9 / 8 / 7 / 9 / 6 / 6) so that a
  change elsewhere can't silently alter what the assistant trusts.

This keeps spoken answers anchored to known publishers rather than arbitrary
search results.

---

## 7. Backups before edits

Before any sensitive script is modified, a **timestamped backup** is made on
disk (e.g. `…backup-before-<change>-<timestamp>`). This means:

- Every change is reversible to a known-good state.
- The history of "what the file looked like before" survives even outside git
  for scripts that live outside a repo.

---

## 8. Validation before push

Nothing ships on a failed check. The standard gate is:

- `bash -n` for shell scripts, `python3 -m py_compile` for Python.
- `tsc` type-check for the frontend.
- JSON validation for the action card and caches.
- **Route simulation** to confirm intended phrases route correctly and existing
  routes are unaffected.
- Confirm invariants (e.g. trusted source counts, no secrets, services healthy)
  *before* committing or pushing.

If a check fails, the change is fixed or rolled back — not forced.

---

## Threat-model summary (home lab)

| Concern                        | Mitigation                                            |
|--------------------------------|-------------------------------------------------------|
| Remote code execution          | Free-agent gateway disabled; Telegram command-only    |
| Untrusted remote input         | Allowlist + fixed handlers; no shell from user text   |
| Secret leakage                 | No secrets in logs/output/UI/docs; terminal-only debug|
| Misinformation / open-web drift| Trusted-source-only briefs; block when unconfigured   |
| Route hijack / collisions      | Narrow triggers; specific-before-broad; simulation    |
| Risky edits                    | Timestamped backups; validate-before-ship             |

> Scope note: this is a single-user home-lab system, not an enterprise
> production deployment. The mitigations above are appropriate to that scope and
> are described honestly as such.
