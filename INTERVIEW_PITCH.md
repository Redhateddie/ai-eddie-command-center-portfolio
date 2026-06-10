# Interview Pitch — AI-EDDIE / Jarvis Command Center

Talking points for interviews. Conversational, honest about scope (home lab),
and grounded in what was actually built and measured.

---

## 30-second pitch

"In my home lab I built AI-EDDIE — a local AI command center on Linux. It's a
voice assistant that speaks short briefs from sources I trust, shows the
supporting links on a browser Portal, and can be checked remotely through a
command-only Telegram bot. The fun part was the engineering around it: I cut the
spoken-response time from around 45 seconds to about 5–7 by profiling each stage,
and I deliberately locked down the remote surface so it couldn't become a remote-
execution risk."

---

## 60-second pitch

"AI-EDDIE is a self-hosted assistant I built and maintain on a Linux machine. It
ties together a wake-word voice loop, a browser status Portal, a text-to-speech
service, and a Telegram bot. When I ask for something like 'cybersecurity news,'
it searches a curated set of trusted sources, posts the results to the Portal,
and speaks a short summary.

Two pieces I'm proud of: first, performance — briefs used to take 35 to 55
seconds before any audio, so I added stage timing, found that an optional
narration step was the bottleneck, and made the fast path the default. First
word now lands in about 5–7 seconds. Second, security — I disabled an open-ended
remote 'agent' gateway and replaced it with an allowlisted, command-only Telegram
interface, so user text never reaches a shell.

It's a home-lab project, not production software, but it's been a realistic way
to practice Linux operations, troubleshooting, and safe change management."

---

## 2-minute technical explanation

"The system is a set of small, decoupled components that talk through JSON.

A wake-word listener transcribes what I say and matches it against an ordered
route table — that table is essentially the control plane. The first matching
route runs its action and exits, so route *order* matters: specific routes come
before broad ones to avoid collisions.

For a live brief, the action runs a trusted-source-biased search, publishes a
JSON 'action card' that the React Portal renders, then speaks a short answer
through a persistent TTS service that plays audio via PulseAudio. Each brief also
writes a JSON cache — category, timestamp, the trusted sources used, the result
titles, the spoken summary. That cache powers conversational follow-ups like
'tell me more' or 'what sources did you use,' which read only from cache and
never hit the network, so they're fast and always consistent with what was just
said.

On latency: the original briefs were slow because they always ran an LLM
narration call that could take up to ~40 seconds. I added optional stage timing —
terminal-only, never spoken — and confirmed the search and parse were ~2 seconds
while narration dominated. So I flipped the default to a fast extractive answer
built from the trusted headlines, and made LLM narration opt-in. Time-to-first-
word went from about 45 seconds to roughly 5–7.

On the UI: the Portal's 'speaking' aura was firing before audio because the
signal was emitted before synthesis. I moved it to the actual playback boundary,
with a fallback that always clears the state, and kept mic gating on a separate
flag so I wouldn't break listening.

And on safety: the remote surface is command-only and allowlisted, there are no
secrets in logs or output, and every change goes through backup, validation, and
route simulation before it ships."

---

## "What was the hardest part?"

"Resisting the urge to guess. The latency and the aura issues both *looked* like
quick fixes, and an earlier aura attempt — a hardcoded delay timer — actually
made things worse because synthesis time varies. The hardest and most valuable
part was forcing myself to measure first: add timing, find the real boundary,
and change that. Once I had the stage breakdown, the latency fix was almost
obvious — the slow narration step just shouldn't be in the default path."

---

## "How did you secure it?"

"Least privilege and no secrets, basically. I disabled the flexible remote
'agent' gateway because it was effectively remote code execution on a personal
machine. Remote access is now a small allowlist of read-oriented commands over
Telegram, and user text never reaches a shell. Secrets — tokens, keys, IDs —
never appear in logs, spoken output, the Portal, or my docs; even the debug
timing prints only to the terminal. Live briefs are trusted-source-only and
block instead of searching the open web if a category isn't configured. And I
back up and validate before every change so nothing risky ships unverified."

---

## "How did you troubleshoot latency?"

"I instrumented it. I added an environment-flag-gated timer that printed how long
each stage took — search, parse, narration, audio — straight to the terminal so
it never affected the spoken output. The baseline showed ~55 seconds total with
narration as almost the entire cost. That told me the fix wasn't to optimize the
search or the audio; it was to take the expensive narration call out of the
default path. I made fast extractive the default, kept LLM narration as an opt-in
flag, then re-ran the timing and re-checked the cache, follow-ups, and source
counts to make sure I hadn't regressed anything."

---

## "What would you improve next?"

"A few things. I'd surface a confirmed broadcaster in the World Cup guide when
the sources clearly state one — right now it honestly says when it can't confirm
a channel, which is safe but could be richer. I'd add lightweight automated tests
around the route table so I'm not relying on manual simulation. And I'd write a
reproducible setup script and some service-hardening notes so the whole thing can
be stood up cleanly on a fresh machine. Longer term, broaden the cached follow-
ups to more categories."

---

_Talking points for a personal home-lab project. Honest about scope; no
enterprise-production claims; no secrets referenced._
