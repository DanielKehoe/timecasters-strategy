# Timecasters Strategy

This repo holds strategy notes, research, and decision records for Timecasters / AstroPrompt. It is not a codebase — expect Markdown documents, not application code.

## The problem being worked on

AstroPrompt has a product-market fit problem: the app is trying to do too many things (financial predictions, news, relationship helper, astrological planner) and users have trouble understanding what it is. That suggests two broad directions:

1. **Spin off focused products** from the current app (e.g., a simple "Bad Astrology" Mercury-retrograde / Moon VOC utility, or a Daily Planner as the flagship).
2. **Rethink the architecture entirely** — move from a standalone app to infrastructure (MCP servers, question classifiers) that lets any LLM answer astrology questions natively, calling specialized calculation engines as needed.

## Go-to-market tension

Two possible paths, each with real drawbacks:

- **Indirect / professional astrologers** (UAC conference, influencer astrologers): gatekept, clique-heavy, and many pros are skeptical of AI for astrology.
- **Direct-to-consumer** (paid ads, podcasters, TikTok/Instagram): simpler audience but requires marketing spend and reach.

Onboarding friction is a persistent problem across both — the app currently requires a video-call training session to use well.

## The deeper question

Under all of the above is a bigger question: is the real opportunity a standalone app at all, or is it infrastructure that lets any LLM answer astrology questions with proper calculations behind them? That framing would shift the work from "build and market AstroPrompt" to "build a question classifier + specialized MCP servers (Astrolog, Swiss Ephemeris) that any AI harness can call."

## Repo layout

- `notes/` — synthesized strategy notes; the primary source of current thinking
- `research/` — market, competitor, technical research
- `interviews/` — notes from conversations with astrologers, users, advisors
- `validation/` — experiments and evidence for/against hypotheses
- `decisions/` — decision records (what was chosen and why)

## How to help

When the user asks for help here, they're usually thinking through strategy, not asking for code. Engage with the ideas: push back on weak arguments, surface tradeoffs, connect new input to prior notes. When writing new documents, match the tone of existing notes in `notes/` — first-person, exploratory, willing to hold multiple hypotheses at once.

## Required header on every new Markdown file

Every new `.md` file in this vault must start with this frontmatter block, before any other content:

```markdown
---
- Version: <increment integer, starting at 1>
- Date: <YYYY-MM-DD>
- Author: <Claude Cowork | Claude Code | Claude Chat | Codex | Perplexity | other>
- Depends on: <previous version, draft, or reference document — or "none">
---
```

- `Version` — integer. New file = `1`. Bump when you substantively rewrite.
- `Date` — today's date in `YYYY-MM-DD`.
- `Author` — which assistant produced the file. Claude Code (this harness) writes `Claude Code`.
- `Depends on` — filename or short reference if the note is derived from or extends another. Use `none` if standalone.

Applies to any new Markdown. When editing an existing file that lacks the header, don't add one retroactively unless the user asks.
