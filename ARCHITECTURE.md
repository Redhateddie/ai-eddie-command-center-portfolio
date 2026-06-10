# Architecture — AI-EDDIE / Jarvis Command Center

> Sanitized technical overview. Diagrams are conceptual; no internal hostnames,
> ports tied to secrets, tokens, or private paths are included.

---

## Components

### 1. Voice listener
Captures microphone audio after a wake word, transcribes it to text, and matches
the text against an **ordered route table**. The first matching route wins and
runs its action, then exits — so route *order* is part of the design. A
microphone gate (a simple on-disk flag) prevents the assistant from hearing its
own speech while talking.

### 2. Portal frontend
A browser HUD (React + Vite, TypeScript, canvas visualization). It shows:
- the latest **action card** (the "live web intel" result list),
- a **speaking-state aura** that reflects when audio is actually playing,
- ambient status.
It reads a small JSON action card and listens for speaking-state events from the
backend.

### 3. Backend API
A lightweight local HTTP service that tracks speaking state and serves
event signals used by the Portal (e.g. "speaking started / stopped"). It is the
glue between what the audio layer is doing and what the Portal shows.

### 4. TTS service
A persistent text-to-speech service (Kokoro voice) that synthesizes text to a
waveform and plays it through the system audio (PulseAudio). The speaking-state
**start/stop signal is emitted around the actual playback call**, so the UI
matches reality. Synthesis logic itself is treated as off-limits for unrelated
changes.

### 5. Telegram command bot
A remote interface that is **command-only**: each supported message maps to a
fixed handler behind an allowlist (status-style, read-oriented actions). User
text is never passed to a shell, and there is no general remote-execution path.

### 6. Trusted live brief system
For a fixed set of categories, runs a **trusted-source-biased search**, publishes
the result as the action card, and speaks a short brief. Source *names* come from
a configured trusted-sources list. If a category has no configured sources, the
brief **blocks** rather than searching the open web. Default narration is **fast
extractive**; LLM narration is opt-in via an environment flag.

### 7. Cache layer
After each brief, a JSON cache records the category, timestamp, trusted sources
used, result titles/subtitles, the spoken summary, and the mode. Caches are
written atomically and contain **no secrets**. A separate cache exists for the
World Cup guide.

### 8. Route controls
The ordered route table is the assistant's control plane. Design rules:
- **Specific routes before broad ones** (e.g. category briefs before any generic
  catch).
- **Narrow triggers** to avoid hijacking (e.g. World Cup fires only on phrases
  containing "world cup").
- **Follow-ups are cache-bound** and never re-search.
- **"stop" always interrupts.**
Route changes are verified with a simulator that replays phrases through the
table in file order and reports the first match.

---

## Data flow examples

### Voice live brief flow

```
  "Hey Jarvis, cybersecurity news"
        │
        ▼
  Wake word ─▶ Listener (transcribe)
        │
        ▼
  Route table ── match: cybersecurity ──▶ live brief script
        │
        ├─▶ trusted-source-biased web search
        │         │
        │         ▼
        │   Action card (JSON) ──▶ Portal renders "live web intel" card
        │
        ├─▶ build FAST extractive answer (top trusted headlines)   [default]
        │      (LLM narration only if opt-in flag is set)
        │
        ├─▶ write cache (JSON: category, sources, titles, spoken, mode)
        │
        ▼
  TTS service ─▶ [signal: speaking start] ─▶ play audio ─▶ [signal: speaking stop]
        │                                                        │
        ▼                                                        ▼
  Spoken brief (~5–7s to first word)                   Portal aura matches audio
```

### Telegram command flow

```
  Phone ─▶ Telegram message ("/status" style command)
        │
        ▼
  Bot receives ─▶ is command in ALLOWLIST?
        │                   │
        │ no                │ yes
        ▼                   ▼
  Ignore / safe reply   Fixed command handler (read-oriented)
                            │
                            ▼
                      Local status gathered (no shell from user text)
                            │
                            ▼
                      Reply with status (no secrets in output)
```

### Cache follow-up flow

```
  "tell me more" / "make it shorter" / "what sources did you use" / "repeat"
        │
        ▼
  Route table ── match: follow-up ──▶ follow-up handler
        │
        ▼
  Read last-brief cache (JSON)   ── NO web search, NO Portal rewrite ──
        │
        ├─ "sources"  ─▶ list cached trusted sources
        ├─ "shorter"  ─▶ condense cached summary/titles
        ├─ "more"     ─▶ expand from cached titles
        └─ "repeat"   ─▶ speak cached summary again
        │
        ▼
  TTS service ─▶ spoken answer (bounded entirely by cache)
```

### World Cup Watch Guide flow

```
  "what World Cup games are today?" / "where can I watch the world cup?"
        │
        ▼
  Route table ── trigger fires ONLY if phrase contains "world cup" ──▶ guide
        │
        ▼
  Trusted-broadcaster-biased web search (default region: U.S. viewing)
        │
        ▼
  Action card (JSON) ──▶ Portal shows results
        │
        ▼
  Detect intent (today / next / schedule / watch)
        │
        ├─ confirmed broadcaster in results ─▶ speak extractive answer
        └─ no confirmed channel             ─▶ honest "schedule found, channel
        │                                      not confirmed; sources on Portal"
        ▼
  Write World Cup cache (JSON, no secrets) ─▶ TTS speaks short answer
```

---

## Cross-cutting design notes

- **JSON as the contract.** The Portal, caches, and follow-ups all communicate
  through small JSON documents, which keeps components decoupled and easy to
  validate.
- **Signals at the boundary that matters.** The speaking aura is driven from the
  real playback boundary, not an upstream guess.
- **Reversibility.** Scripts are backed up before edits; the route table is
  validated by simulation; nothing ships on a failed check.
- **Least privilege for remote.** The only remote surface is a small allowlisted
  command set — not a shell.

---

_Conceptual architecture for a sanitized portfolio. No credentials, private
hosts, or sensitive script internals are included._
