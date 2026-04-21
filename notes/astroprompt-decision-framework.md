---
- Version: 4
- Date: 2026-04-21
- Author: Claude Code
- Depends on: astroprompt-strategy-notes.md, task-planning-codex.md, six-month-strategic-plan-codex.md, astroprompt-baseline-state.md
---

# AstroPrompt Decision Framework

## What this document is

A working framework — not a decision — for developing product-market fit for one or more revenue-positive products from the AstroPrompt / Timecasters opportunity. It synthesizes the 2026-04-21 strategy notes, integrates the Codex task-planning and six-month strategic-plan documents, incorporates baseline-state findings from user-feedback review, and proposes a portfolio plan with a decision matrix, research agenda, pilots, validation criteria, and a timing overlay mapping the plan against astrological conditions May–October 2026.

Per the Codex day-planning analysis, today is inside a long Moon VOC. This document is diagnostic and structural; it commits to no pivot, no spend, no external message. All scoring and recommendations are **provisional until evidence arrives**.

**Version history:**

- **v1:** framed the problem as "pick one wedge." Too narrow.
- **v2:** shifted to a portfolio frame — nested, parallel, and supporting products on a shared substrate.
- **v3:** overlaid the Codex six-month timing analysis and extended the planning horizon to 2026-10-31.
- **v4 (this version):** corrects the PMF diagnosis after user-feedback review. AstroPrompt is in the *first weeks* of a soft rollout, not at the end of a failed launch. The soft rollout becomes the primary evidence-gathering stream. Substrate rework becomes a concurrent first-class work stream. Decision-date semantics shift from a single date to two checkpoints that wait for data maturity.

## Framing correction — too new to conclude PMF failure

Earlier versions treated "AstroPrompt is doing too many things and no one uses it" as an established PMF diagnosis. Review of `user_feedback/` and `astroprompt-baseline-state.md` corrects that. Outreach *just started*. v1.0.2 is newly deployed. The ~11 warm-contact introductions are at varying early stages:

- **Judy** — onboarded 2026-03-02 with strong enthusiasm ("this is so great", "exactly what I needed"); Travel Planner landed particularly well; has asked for a **second session**.
- **Divina** — first real session 2026-04-19 (two days ago); after initial UI struggle she unprompted articulated the USP ("personalized astrological predictions I can't get elsewhere without paying an astrologer $300/hr"); extensive conceptual feedback, no UI feedback; suggested focused apps around "getting laid and getting paid" (love and money).
- **Hollie McKintrick** — two prior sessions; onboarding meeting **not yet held**; strongest professional-astrologer candidate.
- **Dorothy Oja** — invitation sent this week; has not engaged.
- **Other friends (~7)** — various states of introduction, none yet engaged past first touch.

Calling this "PMF failure" conflates two hypotheses:

1. **H1 (PMF failure):** with good onboarding and time, no one retains — the concept doesn't sustain use.
2. **H2 (too new to tell):** no one has had enough exposure cycles yet for retention signal to exist.

The baseline data is consistent with H2, not H1. **We don't actually know yet.** The right response is not a strategic pivot driven by a failure hypothesis — it's a disciplined soft rollout that produces the evidence needed to distinguish H1 from H2.

**Strategic consequence.** The soft rollout of AstroPrompt *is* the evidence sprint. It's not separate from or competing with the "real" research — it's the primary research. The portfolio and substrate work happen concurrently, but the living pilot generates the data that lets a focal product be picked from evidence rather than hypothesis.

## Why portfolio framing

Looking at the six candidates end-to-end, three things remain true:

1. **They compose.** A Moon VOC utility is a natural top-of-funnel for a Daily Planner. A course is a natural upsell above a Planner. An infrastructure layer sits underneath any of them. These aren't competing for the same slot.
2. **They share an asset.** All of them are a thin surface over the same calculation + interpretation engine. Building the engine once and reusing it makes building multiple products cheaper than it looks — *once* the engine has been reshaped for multi-surface reuse.
3. **The real scarce resource is attention, not product count.** Plan the portfolio, sequence it, protect attention through sequencing — don't prematurely kill candidates.

In v4, AstroPrompt itself (A) is understood as one product in the portfolio whose first validation cycle is *already running*. The soft rollout of A produces evidence that informs the rest.

## Pruning the Codex task list

From `task-planning-codex.md`:

- **Playwright for AstroPrompt** — orthogonal to PMF. Revisit once focal is chosen.
- **Discussion forums** — premature.
- **Publishing astrologyprompt.com SEO pages** — premature; outlining fine.
- **Instagram influencer list** — research artifact, not an outreach queue. (Daniel already has ~100 astrology podcasts and ~200 YouTube channels compiled; one outreach has begun.)
- **Course & influencer bundle** — kept as a portfolio component.
- **"Bad Astrology" iOS app** — kept as a portfolio component; three-sentence premise only this window.

Kept and elevated: the strategic review, the product-direction map, the daily-planner thread, and — newly promoted — the **astrology-infrastructure-for-LLMs thesis**, operationalized in v4 as a concrete question-classifier MCP (see Pilot 3).

## Restating the core question

> Given that AstroPrompt is in a just-started soft rollout and the warm-contact cohort hasn't yet generated retention evidence, what **portfolio sequence** — built on a shared substrate — best converts the existing assets (v1.0.2 deployment, Astrolog-backed API, domain expertise, warm relationships) into one or more revenue-positive products, and what disciplined evidence process tells us when we have enough to commit?

Four sub-questions:

1. **Rollout discipline:** how do we run the soft rollout so it produces decision-grade evidence in 6–12 weeks?
2. **Substrate readiness:** what reshaping does the Astrolog API + interpretation layer need to support multiple product surfaces?
3. **Portfolio shape:** given soft-rollout evidence as it arrives, what funnel / upsell / parallel / sibling structure fits?
4. **Channel:** how do users in each product find and trust it?

## Candidate products and their relationships

| #   | Product                              | Role in the portfolio                                                                                                                                                            |
| --- | ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| A   | **AstroPrompt v1.0.2 (current)**     | **Active pilot, primary evidence stream.** Run a disciplined soft rollout; it generates the data that lets us pick a focal product with confidence.                              |
| B   | Daily Planner spin-off               | **Focal product candidate.** Leading hypothesis based on existing signal (Judy's Travel Planner response, Daniel's own usage pattern); to be confirmed by soft-rollout evidence. |
| C   | Moon VOC utility ("Bad Astrology")   | **Top-of-funnel candidate under B.** Free → premium personal VOC → upsell to Planner.                                                                                            |
| D   | Professional astrologer tool         | **Sibling vertical.** Hollie is the best probe here. Distinct customer and roadmap.                                                                                              |
| E   | Astrology infrastructure (MCP layer) | **Parallel bet,** operationalized as a question-classifier MCP server with dual internal (AstroPrompt Learn view) and external (MCP) surfaces.                                   |
| F   | Course + tool bundle                 | **Upsell above B.** Monetizes B's active users; influencer-distributable.                                                                                                        |

Composition relationships:

- **Ladder:** C → B → F. Free utility → paid daily product → high-ARPU course.
- **Parallel:** E runs alongside, sells to a different buyer (developers, AI products).
- **Sibling:** D is adjacent but independent; shares substrate, not roadmap.
- **Pilot under observation:** A is its own live experiment; evidence from A informs everything else.

## The shared substrate

The single most leveraged layer underneath this portfolio is the **calculation + interpretation engine**.

**Important correction from baseline (v4).** Per Daniel, the current Astrolog-backed API was designed for AstroPrompt's feature set and **needs significant reworking** to serve other products. "Shared substrate" is aspirational today, not current. Multi-surface reuse is a work stream that must start before C, D, E, or a spun-off B can lean on it.

**Concrete substrate work (May–August):**

1. **Separate the Astrolog-backed calculation layer from AstroPrompt's feature-specific glue.** Turn it into a clean internal service (versioned API, documented) — not a rewrite.
2. **Build the question classifier as a standalone service** per Daniel's design: a separate and independent API / MCP server with two surfaces:
   - *Internal surface:* an endpoint powering a new AstroPrompt view under the "Learn" grouping — tells the user which view best answers their question, or that the question can't be answered with current datasets.
   - *External surface:* the same endpoint as an MCP server for general use via AI chat platforms.
3. **Treat question classification as its own work item,** feeding A (onboarding friction reduction), B (core interaction loop), and E (public infrastructure probe).

**Hard unresolved issue on E:** distribution. How do AI chat platforms discover and call the classifier? Without a discovery/distribution mechanism the external-utility claim is speculative. Pilot 3 exists partly to test this.

**Practical test** when deciding whether to build a product surface: *does it force the engine to do something new, or just expose existing capability differently?* The latter is almost free; the former needs its own evaluation.

## Provisional decision matrix

| Axis                                 | A: Current AstroPrompt | B: Daily Planner        | C: Moon VOC       | D: Pro tool     | E: Infrastructure       | F: Course+tool    |
| ------------------------------------ | ---------------------- | ----------------------- | ----------------- | --------------- | ----------------------- | ----------------- |
| Clarity of user job                  | L (broad)              | H                       | H                 | M               | L                       | M                 |
| Onboarding friction (lower = better) | H friction             | M                       | L                 | M               | L for devs              | M                 |
| Time-to-first-revenue                | in-flight              | 4–8 wk after focal pick | 8–12 wk           | 8–12 wk         | 12–24 wk                | 8–16 wk           |
| Reuse of existing assets             | H (is the asset)       | H                       | M                 | H               | H                       | H                 |
| Distribution path clarity            | early                  | M                       | M                 | M (Hollie, UAC) | L                       | M–H (influencers) |
| Revenue-per-user ceiling             | M                      | M                       | L                 | H               | M–H                     | H                 |
| Market size                          | M                      | H                       | M                 | L               | unclear, possibly H     | M                 |
| Moat / defensibility                 | L                      | L                       | L                 | M               | M–H                     | M                 |
| **Composition value**                | H (evidence stream)    | H (focal anchor)        | H (funnel into B) | L (sibling)     | M (validates substrate) | H (monetizes B)   |
| Founder energy / fit                 | —                      | H                       | M                 | L               | H                       | M                 |

## Evidence status — what we actually know vs. hypothesize

The matrix is qualitative; the evidence underneath it is asymmetric. Being explicit:

- **A (current AstroPrompt):** live but early. Judy's strong positive response and second-meeting request; Divina's unprompted USP articulation; Hollie's two preliminary sessions. **Zero sustained users yet.** Status: **early soft-rollout data; insufficient to conclude PMF success or failure.**
- **B (Daily Planner):** no direct market test. Supporting signals: Judy was visibly excited by Travel Planner (a planner variant); Daniel uses the Daily Planner himself near-daily; the planner narrative recurs across strategy notes and sessions. Status: **leading hypothesis, not yet validated.**
- **C (Moon VOC utility):** no test. Status: **hypothesis only.**
- **D (Pro tool):** Hollie is a weak positive signal; Dorothy's non-engagement is a weak negative. Status: **hypothesis + small ambiguous signal.**
- **E (Infrastructure MCP):** no test; articulated design exists. Status: **hypothesis only; most strategically interesting, least validated.**
- **F (Course):** no test. Status: **hypothesis only.**

The matrix tilts toward "B as focal, once soft-rollout evidence confirms a planner-shaped job," but that's forward inference, not result.

## Provisional portfolio shape

- **Primary evidence stream (now → ~July):** A's soft rollout, run with discipline and structured observation.
- **Shared substrate work (May–August, concurrent):** Astrolog API layer separation + question classifier as standalone service (internal endpoint + MCP).
- **Focal product (commit when evidence supports):** probably B, possibly a variant the rollout surfaces.
- **Funnel under B:** C — gated on Pilot 1/2 signal.
- **Upsell above B:** F — gated on B having users.
- **Parallel bet:** E — question-classifier MCP probe starts modestly in parallel with substrate work.
- **On hold:** D — revisit if Hollie's arc produces concrete buyer signal.

**Sequencing principle:** one active *consumer* build at a time. Substrate + E can proceed concurrently because they're engine work, feeding multiple products regardless of focal choice.

## Research agenda (revised against baseline)

1. ~~**Who uses AstroPrompt today?**~~ **Answered by baseline: zero retained users; several active-onboarding candidates.** Replaced by **instrument the soft rollout** — standing observation protocol per session.
2. **What jobs do soft-rollout users actually want to perform?** Structured capture during Judy's 2nd session, Hollie's onboarding, Divina's follow-up, Dorothy if she engages, and new weekly contacts. Watch for job clustering.
3. **State of the MCP ecosystem for specialized calculation servers.** Desk research — publishing, discovery, monetization patterns. Open.
4. **Competitive shape of "daily planning with astrology."** Co-Star, The Pattern, Sanctuary, Nebula, Chani. Open.
5. ~~**Is Hollie's audience a beachhead?**~~ Folded into soft-rollout observation — Hollie is a live case, not a research question.

## Pilots / prototypes

### Pilot 1 — "Daily Planner, only" landing test (narrative test for B)

- One-page landing. Tests *words*, not product — does a Daily-Planner-specific framing pull better than the AstroPrompt workbench presentation.
- Signal: conversion traffic → email; free-text "what would you use this for."
- Stop rule: 2 weeks of traffic with conversion well below consumer-productivity benchmarks → B's demand hypothesis weakens.

### Pilot 2 — Soft rollout of AstroPrompt with structured observation (tests A, informs everything)

This is the *primary* evidence stream.

- **Prioritized warm contacts:** schedule Judy's requested 2nd session; complete Hollie's onboarding meeting; Divina follow-up after 2–3 weeks; nudge Dorothy; structured outreach to 2–3 additional warm contacts per week.
- **Observation protocol each session:** which views they choose first; what question they ask unprompted; where they get stuck; how they articulate the USP in their own words; whether they return within 7 days.
- **Instrumentation:** usage data from the existing dashboard (pages viewed, tokens used) + session notes + ~2-week and ~4-week follow-up check-ins per user.
- **Cohort size target:** 10–15 actively-onboarded users by end of June.
- **Stop rule:** by end of July if no more than 1 user is using the app at least weekly unprompted, H1 gains weight and a pivot becomes justified. Until then H2 is the working hypothesis.

### Pilot 3 — Question-classifier MCP probe (tests E)

- Build the question classifier as a standalone service per Daniel's design. Publish as an MCP server. Minimal scope.
- External signal: discovery, pickup, any inbound. Internal signal: does the AstroPrompt Learn-view endpoint cut first-session onboarding friction.
- Stop rule: 6–8 weeks of no MCP pickup *and* no observable onboarding-friction improvement → E is too early or needs reframing.
- **Timing note:** publish the minimal probe in May/June; expansion is an August task if signal appears.

### Pilot 4 — "Bad Astrology" three-sentence premise (tests C, text-only)

Not a build. Premise, free feature, premium feature, upsell to B — one page. Gated on Pilot 1/2 signal.

### Pilot 5 — Course premise one-pager (tests F, text-only)

Not a build. Module outline, student outcome, tool's role, ideal influencer profile — one page. Gated on B having user traction.

### Substrate rework (work stream, not a pilot)

Separate the Astrolog-backed API from AstroPrompt's feature glue; introduce versioning and a clean internal interface; make the calculation layer reusable across surfaces. Runs May–August. Prerequisite for B/C/D/E surface work beyond minimal probes.

## Validation criteria — what would decide this

- **Confirm H1 vs H2 about A:** by 2026-07-31, if ≥2 soft-rollout users return unprompted at least weekly for ≥3 weeks, H2 is confirmed and a planner-shaped job is likely real. If ≤1, H1 gains weight.
- **Confirm B as focal:** soft rollout shows a shared daily/weekly planning job; Pilot 1 landing converts plausibly; Judy's and/or Hollie's ongoing use involves planner views.
- **Confirm C as funnel:** soft-rollout sessions surface spontaneous "check before planning" behavior; Pilot 4 premise lands with 2+ subjects.
- **Confirm E as parallel bet:** Pilot 3 gets organic pickup; research question 3 shows viable discovery/monetization patterns; Learn-view endpoint measurably reduces onboarding confusion.
- **Confirm F as upsell:** decidable only after B has a user thread.
- **Revisit D:** Hollie produces a specific paid offer she'd recommend to clients.

**Decision cadence (v4 revision).** The old "decision on 2026-06-02" was too rigid given the rollout context. Use **two checkpoints**:

- **2026-06-02 (end of 6-week sprint):** review state of soft rollout, substrate progress, Pilot 1/3 signal. *Either* commit to a focal product *or* explicitly extend the rollout by 4–6 weeks with sharpened instrumentation. Not committing is a legitimate outcome if evidence is thin.
- **2026-07-31 (end of Codex channel month):** second checkpoint. By this date the H1-vs-H2 question about A should be answerable. Commit to a focal product here, or acknowledge a real pivot is warranted.

The point is to avoid both premature commitment and indefinite research.

## Timing overlay (May–October 2026)

Codex's six-month staging maps to this portfolio:

| Month     | Conditions                                               | Best use for this portfolio                                                     | Caution                                                |
| --------- | -------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------ |
| May       | Mars/Jupiter pressure; Saturn trines supportive          | Soft rollout sessions + observation; start substrate rework; minimal MCP probe  | Over-pivoting from single session reactions; paid ads  |
| June      | Jupiter squares Sun/Mercury; Saturn trine Venus          | More rollout sessions; onboarding-flow refinements; positioning narrative tests | Overclaiming; confrontational pro conversations        |
| July      | Jupiter enters 7th; Saturn opposite Jupiter              | Focal-product scoping; channel / UAC decision; substrate near-complete          | Overcommitting; signing partnerships                   |
| August    | Mars trines; Jupiter conjunct Uranus; **eclipse Aug 28** | **Strongest build window** for the chosen focal product                         | Broad launch near eclipse; multiple pilots in parallel |
| September | Mars squares; Pluto square Neptune                       | Beta with focal-product MVP; UAC strictly as research                           | Major launch; aggressive outreach                      |
| October   | Jupiter sextile Moon; Jupiter square MC                  | One bounded partnership/channel pilot; course test; six-month synthesis         | Inflated claims; abrupt partner reversals              |

**How I'm using this:** as sequencing heuristic over the soft-rollout + substrate-rework + focal-build arc, not a hard gate. Where evidence conflicts with timing, evidence wins. The default staging respects the Codex windows because they align with sensible practice anyway.

## Time-boxed plan

### This week (2026-04-21 → 2026-04-27) — set up the sprint

- Today (VOC): this document. Draft Pilot 1 landing copy. Draft Pilot 2 observation-protocol template. No external commitments.
- After VOC ends 2026-04-22 12:00am Asia/Saigon:
  - **Contact Judy** to schedule her requested 2nd session.
  - **Schedule Hollie's** onboarding meeting.
  - **Send Divina** a 1–2 question follow-up grounded in her 2026-04-19 session — specifically: *what would get her to open the app again on her own*.
  - Gentle nudge to Dorothy (no pressure).
  - Publish Pilot 1 landing.
- By end of week: observation-protocol template ready; substrate-rework scoping note drafted.

### Weeks 2–3 (2026-04-28 → 2026-05-11) — early May: run the rollout, start the engine work

- Complete Judy's 2nd session; complete Hollie's onboarding; 2–3 new warm-contact sessions.
- Apply the observation protocol; capture notes per user.
- **Start substrate rework** — separating the calculation layer from AstroPrompt's feature glue.
- **Start the question-classifier design** as a standalone service.
- Finish research questions 3 (MCP ecosystem) and 4 (competitive scan) — desk work in parallel.
- Write Pilot 4 and Pilot 5 one-pagers.

**Timing note (May early).** Codex's "diagnose and reduce scope" maps onto soft-rollout observation. Mars pressure argues against over-pivoting from single session reactions; Saturn trines support structural work (the substrate rework).

### Weeks 4–6 (2026-05-12 → 2026-06-02) — late May: sharpen or commit

- Continue rollout (2–3 new sessions per week); 2-week follow-ups with earliest cohort.
- Substrate rework: calculation layer separated and documented.
- Question-classifier: minimal service running; internal Learn-view endpoint in test.
- Pilot 1: 2 weeks of landing-page data reviewed.
- **2026-06-02 checkpoint:** commit to a focal product *or* explicitly extend the rollout with sharpened instrumentation. Document the decision.

**Timing note (May late).** Saturn trines support writing the system down and making structural commitments. Codex flags *broad relaunch, paid ads, simultaneous spin-off products* as ill-advised in May — none are in this plan.

### June 2026 — positioning, more rollout, more substrate

- Continue soft rollout; aim for 10–15 actively-onboarded cohort by end of month.
- If B is confirmed focal: positioning drafts + first-session onboarding flow design.
- Substrate rework: question classifier stable; MCP surface published minimally.
- Narrative tests on Pilot 1 landing: "AI daily planner" vs. "Moon VOC alerts" vs. other.
- **Do not start the focal-product consumer build yet.**

**Timing note (June).** Jupiter squares Sun/Mercury → overclaiming risk in positioning. Saturn trine Venus supportive of making the offer attractive. Good month for narrative + design; poor for confrontational pro conversations.

### July 2026 — scope, channel, second checkpoint

- Focal-product MVP spec finalized (surface, substrate dependencies, metrics).
- Channel/partnership map complete; UAC decision on evidence — research-only or skip.
- Influencer list stays a research artifact; no cold pitches.
- **2026-07-31 checkpoint:** the H1-vs-H2 question about A should be answerable by this date. Commit to a focal product, or acknowledge a real pivot is warranted.

**Timing note (July).** Saturn opposite Jupiter = realism check. Honest channel analysis; no partnership signings.

### August 2026 — build the focal-product MVP

- Primary execution window.
- If focal = B: thin Daily Planner surface over the (now-reshaped) substrate; question classifier as the core interaction; packaging for beta.
- If focal = E: expanded MCP server from the probe, with documentation and first developer audience.
- Pilot packaging: demo script, landing v2, beta cohort list, activation/repeat-use metrics.
- **No public launch around 2026-08-28 (partial lunar eclipse).**

**Timing note (August).** Strongest execution window. Use it. Avoid mid-month scope creep ("we could also ship C").

### September 2026 — field learning, no launch

- Beta cohort uses the MVP; structured objection and failure collection.
- If UAC mid-September: research + relationship mapping only. Demo concisely. Do not pitch.
- Revise matrix and portfolio shape.

**Timing note (September).** Mars squares + Pluto square Neptune = volatile. Field research thrives; commitments don't.

### October 2026 — partnership pilot and synthesis

- One bounded partnership/channel test (one astrologer, *or* one influencer, *or* one teacher, *or* one infrastructure collaborator — one).
- If MVP held up: small course/workshop experiment (F's first test).
- Six-month synthesis.

**Timing note (October).** Jupiter square MC = risk of inflated public claims. Scope-limit October commitments.

### Beyond October 2026

- Cleaner public launch likely November or later.
- E becomes a named work stream if Pilot 3 + any Aug–Sep follow-on signaled pickup.
- D revisited only with concrete buyer signal.

## What explicitly NOT to do in this window

- Don't commit to killing AstroPrompt. It *is* the active pilot and the primary evidence stream.
- Don't declare PMF failure until H1 can be distinguished from H2 with data (target 2026-07-31).
- Don't buy UAC travel yet. July checkpoint at the earliest.
- Don't cold-pitch influencers. Keep the podcast/YouTube list as a research artifact.
- Don't start a Swiss Ephemeris rebuild. Reshape the Astrolog API layer first.
- Don't publish new SEO pages until focal is chosen *and* positioned.
- Don't design the course beyond a one-pager before B has users.
- Don't try to build the portfolio in parallel. Substrate + question classifier is concurrent with rollout; *surface* builds are sequential.
- Don't start the focal-product consumer build in June or July. August is the build window.
- Don't plan a launch around 2026-08-28 or during September.
- Don't over-pivot from a single surprising session reaction, especially during Mars oppositions in early May.
- Don't confuse timing guidance with hard gates. Evidence beats timing when they conflict.

## Open questions (post-baseline)

- **Distribution for the MCP classifier:** how do AI chat platforms actually discover and call specialized MCP servers? Without this, E's external-utility claim is speculative. Targeted research (part of research question 3).
- **Soft-rollout capacity:** how many active-onboarding sessions per week can Daniel realistically run while doing substrate rework? Determines how fast the cohort reaches 10–15.
- **Monetization trigger:** at what soft-rollout signal does it make sense to switch on the 30-free-queries paywall? Too early depresses signal; too late leaves willingness-to-pay untested.
- **Katie Bohinc meeting:** she and Daniel have exchanged LinkedIn messages; she's asked for a meeting with unstated agenda. Plausible framings: beta user, competitor, collaborator, peer, or scouting to hire Daniel for her startup. Go in curious and listen before positioning; don't share strategy or substrate thinking beyond what's already public.

## Key principle

> Plan the portfolio; run the soft rollout as the primary evidence stream; build the substrate concurrently; commit to a focal product when the data supports it — not before, and not based on a failure hypothesis the data doesn't yet support.

AstroPrompt is not a failed experiment. It's a live pilot whose first real sessions were March 2 (Judy) and April 19 (Divina). The response is a disciplined six-month arc — **May** rollout + substrate, **June** positioning + more rollout, **July** scope + commit, **August** build, **September** field learning, **October** partnership pilot — with two honest checkpoints (2026-06-02 and 2026-07-31) at which the portfolio either narrows to a focal product on evidence, or the rollout is extended with sharper instrumentation.
