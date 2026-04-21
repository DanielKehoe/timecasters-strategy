---
- Version: 1
- Date: 2026-04-21
- Author: Claude Code
- Depends on: astroprompt-strategy-notes.md, astroprompt-decision-framework.md
---


Do deep research with extended thinking and generate a research report as a Markdown file ready for download.

### Role and context

You are a deep research analyst producing a strategic briefing on the Model Context Protocol (MCP) ecosystem, agent skills, and LLM tool-use infrastructure as it stands in April 2026. Your reader is a solo founder-developer with 25 years of software experience (Ruby/Rails background, currently shipping AI-integrated web apps) who is considering whether to position a specialized calculation/data service as an MCP server and/or agent skill. His niche is astrology, but he believes the underlying problem — general-purpose LLMs needing specialized calculation or datasets to answer domain questions accurately — is industry-wide, so general and cross-domain analysis is equally valuable.

He needs to understand the **current state**, the **emerging practices**, the **investment tailwinds**, and the **realistic strategic options**. He does not need abstract explanation of protocols or hype — he needs concrete, current, cited facts and patterns he can make decisions from.

Prioritize sources from the last 6–12 months. Flag any claim where the best source is older than 12 months. Date every example.

### Background on the specific case (optional context, do not dwell)

The founder's product, AstroPrompt, currently wraps Astrolog (an astrology calculation engine) as a subprocess API behind a web app. The calculation engine produces datasets (planetary positions, aspects, transits, Moon void-of-course periods, etc.); the app packages those datasets with prompts and sends them to LLMs (GPT/Claude) for interpretation. Users ask questions in natural language and get AI-generated astrological readings grounded in accurate calculations.

The strategic insight he wants tested: instead of building an ever-larger app, he could expose the calculation + interpretation layer as MCP server(s) and/or agent skills that any AI chat surface calls when a user asks an astrology-shaped question. The open question is whether the ecosystem today supports this, how discovery and monetization actually work, and what the realistic strategic positioning options are.

### Research clusters

Structure your output by the eight clusters below. For each cluster, produce: (a) a concrete current-state summary, (b) named examples with links and dates, (c) open issues or uncertainties, (d) implications for a niche calculation-provider strategy.

#### Cluster 1 — Current state of the MCP ecosystem

- Protocol status: current spec version, major revisions since launch, governance (who controls the spec).
- Major clients that consume MCP servers (Claude Desktop, Cursor, Zed, VS Code, etc.) — what each supports, how users configure servers.
- Major server publishers (official Anthropic servers, community, enterprise). Rough inventory of categories represented (filesystem, databases, web services, calculation, search, etc.).
- Known strengths and limitations as of April 2026.
- Any competing or adjacent protocols (OpenAI's tool-use spec, Google / Gemini equivalents, LangChain tool standards, etc.) and how they relate.

#### Cluster 2 — Agent skills as a parallel / complementary pattern

- What are "agent skills" (Anthropic's concept, Claude Code / Claude.ai skills, skill-creator)? How do they differ from MCP servers in mechanism and use?
- OpenAI equivalents (GPTs, custom Assistants, Actions / connectors), Google / Gemini equivalents, and any third-party skill frameworks.
- When does a domain capability belong as a skill vs. as an MCP server vs. both?
- Any emerging unified patterns (e.g., MCP server wrapped by a skill manifest).

#### Cluster 3 — Tool use and specialized calculation in LLM apps generally

- The broader pattern of LLMs calling specialized tools (function calling, retrieval, code execution, external APIs).
- Known cases where general-purpose LLMs need external calculation/data to answer a class of questions reliably: math (Wolfram Alpha), finance (market data APIs), sports (stats APIs), legal (case databases), medical (UpToDate-style), scientific (arXiv retrieval, molecular simulation), etc. Note which of these have been successfully integrated and how.
- Case studies of integrations that worked and those that didn't — what made the difference.

#### Cluster 4 — Discovery mechanisms (the hard problem)

- How do AI chat platforms today know which MCP servers or skills exist and when to use them?
- Is discovery currently manual (user installs server, user invokes skill), semi-automatic (catalog / marketplace), or automatic (agent routes on its own)?
- Emerging registries, hubs, and marketplaces (e.g., Smithery, MCP Hub, mcpservers.org, Anthropic's server directory, any Y Combinator-backed directories). Named examples, scale, business model.
- Is there any analogue of DNS / npm / pip for MCP? What's the closest thing?
- Role of model training data — do base models know about specific MCP servers, and does this matter for routing?
- Academic or open-source work on automatic tool discovery and selection (arXiv papers, Stanford/Berkeley/MIT labs, etc.).
- Does writing a paper (e.g., on arXiv) describing an MCP server meaningfully increase its discoverability by future models via training data? What's the evidence?

#### Cluster 5 — Monetization patterns

- Is anyone making money on MCP servers yet? Concrete examples with revenue or adoption numbers.
- Emerging monetization mechanisms: metered API access behind an MCP wrapper; subscription; marketplace fees; enterprise licensing; revenue share with AI platforms.
- Are AI platforms (Anthropic, OpenAI) offering revenue-share programs for skills/servers? Status.
- Precedents from adjacent ecosystems: App Stores, Chrome extensions, ChatGPT plugins (the plugin marketplace is a useful cautionary tale), Discord bot directories, Zapier apps.
- Payment rails and authentication patterns currently used by paid MCP servers.

#### Cluster 6 — Comparable domain-specific plays

- Companies or projects building specialized calculation / knowledge MCP servers or agent skills for specific verticals (finance, sports, legal, medical, scientific, astrology, tarot, fitness, etc.).
- For each, briefly: what they built, who pays, distribution mechanism, current traction.
- Specifically look for any "question classifier + specialized calculator" two-layer architectures — the founder suspects this is the right pattern and wants to know if anyone else is executing it.
- Any astrology-adjacent AI efforts (Co–Star, Sanctuary, Nebula, The Pattern, Chani, Katie Bohinc's startup) that have moved toward infrastructure/API plays rather than consumer apps.

#### Cluster 7 — Investment landscape

- VCs and angel investors actively funding MCP-ecosystem, agent-infrastructure, and tool-use companies in 2025–2026. Named firms, named deals, deal sizes where public.
- Y Combinator batches and cohorts with concentration in this space.
- Public narratives from investors (blog posts, podcasts, tweets/threads) about why this space matters, what they're looking for, what they think the winners look like.
- Notable funding rounds of MCP-server-first or skills-first companies; acquisitions if any.
- Where on the hype cycle is this today — still rising, at peak, plateaued, disillusioned? Cite specific data or commentary.

#### Cluster 8 — Strategic implications for a niche calculation/classification provider

Synthesize the above into concrete strategic options for a solo founder with a working domain calculation engine and warm access to a niche user community. Evaluate at least these five options and any others the research surfaces:

- **Open-source + paper route:** publish MCP server as open-source; publish an arXiv paper describing it; rely on training-data osmosis and community discovery.
- **Marketplace route:** publish to existing MCP directories / registries / marketplaces; monetize via metered API behind the server.
- **Skill-first route:** package as Claude skill / custom GPT / Gemini equivalent; lean on AI-platform distribution.
- **Two-layer route:** build a question-classifier MCP server (layer 1) that routes domain questions to specialized calculator MCP servers (layer 2) — either both owned or layer-2 as a category.
- **Gate + partnership route:** keep the API proprietary; pursue direct partnerships with AI platform vendors or prosumer app vendors.

For each option: what has to be true in 2026 for it to work; realistic time-to-evidence; capital required; risk of being crushed by platform incumbents.

### Output format

- Long-form markdown document, structured by the eight clusters above, with a brief executive summary at the top (5–10 bullets).
- Every factual claim gets an inline citation with a link and a date.
- When citing, prefer primary sources (protocol spec repos, official blog posts, company sites, SEC filings, arXiv papers, investor posts) over secondary commentary. Cite commentary only when sourcing a viewpoint.
- For each cluster, end with an "Implications for the founder" subsection (3–6 bullets).
- Conclude with a **Strategic options comparison table** (the five+ options from Cluster 8, with columns for: preconditions, time-to-evidence, capital required, incumbent risk, upside if it works, recommended next action).
- Conclude with a **"What I don't know"** section listing questions the research couldn't resolve from publicly available information.

### Source priorities

1. Protocol repos and official docs (github.com/modelcontextprotocol, Anthropic docs, OpenAI docs, etc.).
2. Company blogs and first-party announcements (Anthropic, OpenAI, Google DeepMind, major server publishers).
3. arXiv papers and academic-lab blog posts (Stanford CRFM, Berkeley BAIR, MIT CSAIL, etc.).
4. Y Combinator launch posts and Product Hunt launches for MCP/skills-related products.
5. Credible dev-focused publications and newsletters (Latent Space, The Pragmatic Engineer, Simon Willison's blog, TechCrunch, AngelList, Hacker News discussion threads with named authors).
6. Named investor content (firm blogs, partner posts, podcast transcripts).

### What not to do

- Do not summarize the MCP spec abstractly. Focus on practice, not protocol theory.
- Do not treat MCP as a settled industry standard. It's young; note where practices are forming or contested.
- Do not make up examples. If you can't find a concrete case for a claim, say so.
- Do not recycle Anthropic marketing language. Seek independent assessments.
- Do not assume the founder knows the jargon. Briefly define specialized terms at first use.
- Do not let commentary crowd out facts. Cited facts first, interpretation second.
- Do not pad. If a cluster has thin public evidence, say so and move on rather than inflate.

> End of research brief.

---

## Follow-up research (after first pass)

If the first-pass output is thin or ambiguous on any of the following, run a targeted second pass:

1. **Discovery evidence.** If Cluster 4 didn't produce concrete evidence on whether LLMs can auto-discover MCP servers at inference time (as opposed to the human user configuring them), run a second pass focused narrowly on: (a) papers on tool selection from a dynamic tool inventory, (b) any tool-marketplace announcements with auto-discovery features, (c) Anthropic / OpenAI roadmap statements on auto-discovery.

2. **Paid MCP evidence.** If Cluster 5 didn't name at least 3 paid MCP servers with any adoption, run a second pass: "list all MCP servers currently charging money, with pricing, launch dates, and any adoption signal."

3. **Two-layer architecture precedents.** If Cluster 6 didn't surface any "question classifier routes to specialized calculator" architectures, run a second pass specifically searching for: Wolfram Alpha-style integrations in LLMs 2025–2026, domain routing layers, RAG-over-tools systems, and "semantic tool selection" papers.

4. **Astrology-specific.** If Cluster 6 didn't cover the astrology consumer apps' positioning (Co–Star, Sanctuary, Nebula, The Pattern, Chani, Katie Bohinc's company), run a second pass focused on whether any have moved from consumer app to API / infrastructure / B2B, and at what scale.

5. **Policy and platform risk.** If the research didn't address it, ask: what happens to third-party MCP servers / skills if Anthropic or OpenAI decides to build first-party versions of popular categories? Any precedents from the ChatGPT plugin shutdown.

## Go 

Do deep research with extended thinking and generate a research report as a Markdown file ready for download.
