---
- Version: 1
- Date: 2026-04-21
- Author: Claude Code
- Depends on: astroprompt-decision-framework.md
---

# AstroPrompt Baseline State — 2026-04-21

Answers to the open questions in `astroprompt-decision-framework.md` v3, captured here so the strategic plan and any future revisions can reason from facts rather than assumptions. Snapshot of current state at the start of the 6-week evidence sprint.

## Revenue state

- **Completely pre-revenue.** Zero paid subscribers. No committed active users other than Daniel.

## Runway and time budget

- **Full-time availability.** Daniel is working on this as his full-time focus.
- **Living expenses covered** by existing revenue from Mac Install Guide (currently a single advertiser: Warp).
- Implication: time is not the scarce resource. Attention, focus, and decision-quality are.

## Team

- **Solo.** No co-founder, no contractors, no advisors in a working capacity. Daniel is sole developer and founder.

## Substrate readiness (Astrolog-backed API)

- Currently designed for AstroPrompt's feature set; **needs significant reworking to serve other products**.
- The "shared substrate" described in the decision framework is aspirational, not current. Multi-surface reuse requires deliberate reshaping before downstream products can lean on it.

## Question classification — Daniel's intended design

- A **separate and independent API / MCP server**, not an AstroPrompt feature with external hooks.
- **Internal surface:** an endpoint powering an AstroPrompt view under the "Learn" grouping. Tells the user which AstroPrompt view best answers their question, or that the question can't be answered with an AstroPrompt dataset.
- **External surface:** the same endpoint as a utility for general use via AI chat platforms.
- **Hard open question:** distribution — how AI chat platforms discover and call this classifier. Without a distribution mechanism, the external-utility claim is speculative.

## Active / prospective user state

AstroPrompt v1.0.2 is deployed with a full feature set. Outreach just began.

- **Engaged active users: 0** (other than Daniel himself).
- **11 friends introduced.** None has used it more than once.
- **Judy** — avid amateur astrologer. Asked Daniel to schedule a **second meeting** to show her how to use the app again. Engagement intent present; repeat usage not yet.
- **Divina** — expert amateur astrologer. One onboarding meeting. Offered **extensive conceptual feedback**, but not UI feedback. Unclear whether she'll become an active user.
- **Dorothy Oja** — working professional astrologer, high status in the astrological community. Received invitation this week. **Has not engaged.**
- **Hollie McKintrick** — professional astrologer with strong AI interest; strongest candidate on the professional channel. Responded to invitation last week. **Two sessions so far**; onboarding meeting not yet held.

## Data sources for deeper investigation

- **Insforge Admin MCP server** (available to Claude Desktop) — can query the AstroPrompt database for actual usage data.
- **`/Users/daniel/Workspace/astroprompt-project/user_feedback`** — folder of user feedback notes.
