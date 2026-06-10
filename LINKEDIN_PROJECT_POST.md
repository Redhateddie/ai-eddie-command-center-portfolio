# LinkedIn Project Post

> Draft post — professional, hands-on tone. Honest about scope (home lab). No
> exaggeration, no enterprise-production claims. Edit freely before posting.

---

## Version 1 (concise)

I spent the last stretch building **AI-EDDIE**, a local AI "command center" that
runs entirely on my own Linux machine in my home lab.

It's a voice assistant with a twist: instead of leaning on a slow cloud round-
trip or shallow canned replies, it pulls short spoken briefs from a curated set
of trusted sources, shows the supporting links on a browser Portal, and can be
checked remotely through a **command-only** Telegram bot.

A few things I'm happy with:

🔹 **Latency.** Spoken briefs used to take ~35–55 seconds before any audio. I
added stage-level timing, found a slow narration call was the bottleneck, and
made a fast path the default — time-to-first-word dropped from about 45 seconds
to roughly 5–7.

🔹 **Security by default.** I disabled an open-ended remote "agent" gateway and
restricted remote access to a small allowlist of read-oriented commands. A
personal assistant should expand what *I* can do, not what an attacker can.

🔹 **Boring reliability.** Every change followed the same loop: back up, edit,
validate (syntax, types, JSON, route simulation), then document. Nothing shipped
on a failed check.

It's a home-lab project, not production software — but it's been a great way to
practice the day-to-day reality of Linux operations: running services,
diagnosing performance, making security trade-offs explicit, and keeping changes
reproducible.

Happy to talk through the architecture or the troubleshooting if anyone's
interested.

#Linux #HomeLab #DevOps #Python #Troubleshooting #Security

---

## Version 2 (slightly longer, story-driven)

**What I learned building a voice assistant I actually trust.**

In my home lab I built **AI-EDDIE**, a local AI command center on Linux: wake-
word voice control, a browser status Portal, a text-to-speech service, a
command-only Telegram bot, and a "live brief" system that speaks short news and
sports summaries from sources I curate.

The interesting work wasn't the features — it was the engineering around them.

When briefs felt slow (~35–55 seconds to first sound), I resisted guessing.
I added timing to each stage, saw that a narration step was doing almost all the
waiting, and changed the default so the assistant speaks a fast extractive
answer immediately, with the slower path available only on demand. First word
now lands in about 5–7 seconds instead of ~45.

When the Portal's "speaking" indicator looked out of sync, the real issue was
that the signal fired before audio even started — so I moved it to the actual
playback boundary instead of trying to patch it with a timer.

And on the security side, I deliberately removed a flexible remote-control path
and replaced it with an allowlisted, command-only interface. Less convenient in
theory, much safer in practice.

This is a personal/home-lab project, not enterprise production — but it's taught
me a lot about service design, methodical troubleshooting, and shipping changes
safely (backup, validate, document, repeat).

If you work in Linux/cloud support, DevOps, or AI operations, I'd love to
connect.

#Linux #Python #HomeLab #SRE #DevOps #AI #Security #Troubleshooting

---

_Both versions are drafts for a personal project and intentionally describe it
as a home lab. No secrets, tokens, or private details are referenced._
