---
- Version: 1
- Date: 2026-04-21
- Author: Claude Code
- Depends on: research/prompt-mcp-ecosystem-research-plan.md
---

# MCP Ecosystem Strategic Briefing — April 2026

*Scope: the state of Model Context Protocol, Agent Skills, and LLM tool-use infrastructure, and the realistic strategic options for a solo founder (AstroPrompt) considering whether to expose a specialized astrology calculation engine as MCP server and/or agent skill.*

---

## Executive summary

1. **MCP is now the de facto cross-vendor tool protocol.** Claude, ChatGPT (Apps SDK, Oct 2025), Copilot, Cursor, Windsurf, Zed, Continue, Mistral all consume it. In December 2025 the protocol was donated to the **Agentic AI Foundation** under the Linux Foundation, co-founded by Anthropic, Block, and OpenAI — removing single-vendor capture risk ([Simon Willison, 2025-12-09](https://simonwillison.net/2025/Dec/9/agentic-ai-foundation/)). Current spec: **2025-11-25**.
2. **Agent Skills went cross-vendor in ~6 months.** What launched as Claude Skills (Oct 16, 2025, [claude.com/blog/skills](https://claude.com/blog/skills)) is now the **Agent Skills open standard** at [agentskills.io](https://agentskills.io) — supported by Claude, Codex, Gemini CLI, Copilot, VS Code, Cursor, and ~30 other tools. Skills and MCP servers are complementary, not competing: skill = procedural knowledge + scripts; MCP = networked capability.
3. **Discovery is still a human act.** No production system has a base model discovering and installing unknown MCP servers mid-conversation. Users pick, install, configure. The "unknown-tools problem" on MCP-Universe (Aug 2025) shows even frontier models score 29–44% on unfamiliar servers ([arXiv 2508.14704](https://arxiv.org/abs/2508.14704)).
4. **Token cost is the dominant MCP failure mode.** GitHub's official MCP consumes ~55,000 tokens (30% of context) just advertising tools. Anthropic has already begun steering developers toward **code-execution-over-tools** (Nov 4, 2025) and **skills with progressive disclosure** as the counter-pattern ([Simon Willison, 2025-11-04](https://simonwillison.net/2025/Nov/4/code-execution-with-mcp/)).
5. **No public revenue share exists from any major platform** as of April 2026. Anthropic's Claude Partner Network ($100M, Mar 12 2026, [anthropic.com](https://www.anthropic.com/news/claude-partner-network)) is enterprise-services, not developer revenue-share. OpenAI Apps SDK has a "monetize your app" section with no published percentage. Plan to run your own billing via Stripe Agent Toolkit.
6. **The standalone-MCP-server business is unproven.** Three sustained searches could not name **three paid-at-the-tool-call MCP servers** with public pricing and adoption. The money today flows upstream (existing SaaS wrapping its paid API) or at the hosting layer (Glama, Smithery, Cloudflare).
7. **Wolfram is the canonical comp.** February 2026 launch of "Wolfram as a foundation tool for LLM systems" — MCP Service, Agent One API (drop-in LLM+tool replacement), and CAG Component APIs — is the clearest productized two-layer "LLM + specialized calculator" offering in the market ([Stephen Wolfram, Feb 2026](https://writings.stephenwolfram.com/2026/02/making-wolfram-tech-available-as-a-foundation-tool-for-llm-systems/)).
8. **The astrology vertical is uncontested whitespace.** Co-Star, Sanctuary, Nebula, Chani, The Pattern, Kasamba all stayed B2C. Swiss Ephemeris / Astrodienst remains free-licensed. No astrology-brand has published a B2B API or MCP product as of April 2026. Whitespace cuts both ways — it's also unvalidated demand.
9. **Two-layer "classifier + specialized calculator" is a published, investable pattern.** RAG-MCP (May 2025, [arXiv 2505.03275](https://arxiv.org/abs/2505.03275)) reports a 3x tool-selection-accuracy improvement and &gt;50% token reduction via semantic retrieval. a16z's MCP deep dive names this layer explicitly. Wolfram Agent One is the productized version.
10. **Platform risk is category-specific.** Anthropic and OpenAI will absorb horizontal categories (filesystem, GitHub, calendar) — precedent: ChatGPT plugins deprecated April 2024 after ~13 months, GPT Store rebuilt into Apps SDK (Dec 2025). They will **not** absorb narrow verticals like astrology. The vertical-narrowness of your wedge is your best structural protection.

---

## Cluster 1 — Current state of the MCP ecosystem

### Protocol status and governance

**Current spec version: `2025-11-25`.** MCP uses date-based version strings (`YYYY-MM-DD`) that change only on backward-incompatible revisions. Revision history since November 2024 launch:

| Date           | Key changes                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **2024-11-05** | Initial public launch. HTTP+SSE transport, tools, resources, prompts, sampling ([spec launch announcement](https://www.anthropic.com/news/model-context-protocol), 2024-11-25)                                                                                                                                                                                                                                                                                                                                   |
| **2025-03-26** | OAuth 2.1 authorization framework, Streamable HTTP transport replacing HTTP+SSE, JSON-RPC batching, tool annotations (read-only/destructive), audio content type ([changelog](https://modelcontextprotocol.io/specification/2025-03-26/changelog))                                                                                                                                                                                                                                                               |
| **2025-06-18** | Removed JSON-RPC batching; structured tool output; classified servers as OAuth 2.1 Resource Servers with protected-resource metadata; mandated RFC 8707 Resource Indicators to block token-theft; added **elicitation** (servers can ask users for input mid-call); resource links in tool results; `MCP-Protocol-Version` HTTP header ([changelog](https://modelcontextprotocol.io/specification/2025-06-18/changelog))                                                                                         |
| **2025-11-25** | OpenID Connect Discovery 1.0 for auth-server discovery, icons metadata on tools/resources/prompts (SEP-973), incremental scope consent via `WWW-Authenticate` (SEP-835), URL-mode elicitation (SEP-1036), tool calling *inside* sampling (SEP-1577), OAuth Client ID Metadata Documents (SEP-991), experimental **tasks** primitive for durable/deferred requests (SEP-1686), JSON Schema 2020-12 default dialect, SDK tiering ([changelog](https://modelcontextprotocol.io/specification/2025-11-25/changelog)) |

**Governance.** MCP was moved into the Linux Foundation as **"Model Context Protocol a Series of LF Projects, LLC"** ([governance doc](https://modelcontextprotocol.io/community/governance), Apr 2026). Structure is BDFL-style, explicitly modeled on Python/PyTorch:

- **Lead Maintainers (BDFL)**: David Soria Parra, Den Delimarsky
- **Core Maintainers** (8): Peter Alexander, Caitie McCaffrey, Kurtis Van Gent, Clare Liguori, Paul Carleton, Nick Cooper, Nick Aldridge, Che Liu
- **Emeritus**: Justin Spahr-Summers (co-inventor)
- Code/spec Apache 2.0; docs CC-BY 4.0
- Core Maintainers meet bi-weekly; governance is **individual, not corporate** — no reserved company seats
- Proposals flow through **SEPs** (Specification Enhancement Proposals) via GitHub ([SEP Guidelines](https://modelcontextprotocol.io/community/sep-guidelines))

MCP also sits under the **Agentic AI Foundation**, announced December 2025 under the Linux Foundation; founding platinum members reportedly include AWS, Anthropic, Block, Bloomberg, Cloudflare, Google, Microsoft, and OpenAI ([Simon Willison, 2025-12-09](https://simonwillison.net/2025/Dec/9/agentic-ai-foundation/)). *(Flagged: the exact founder roster was corroborated via secondary coverage; primary LF press release was not retrievable during this research session.)*

### Major clients

Feature notation: T = Tools, R = Resources, P = Prompts, S = Sampling, E = Elicitation.

| Client                | T   | R       | P       | S       | E       | Notes                                                                                                                          |
| --------------------- | --- | ------- | ------- | ------- | ------- | ------------------------------------------------------------------------------------------------------------------------------ |
| **Claude Desktop**    | yes | yes     | yes     | partial | partial | Reference client ([quickstart](https://modelcontextprotocol.io/quickstart/user))                                               |
| **Claude Code**       | yes | yes     | yes     | yes     | yes     | Most complete MCP consumer in Anthropic's stack                                                                                |
| **Cursor**            | yes | yes     | yes     | **no**  | yes     | One-click install from Cursor Marketplace; stdio/SSE/Streamable HTTP with OAuth ([docs](https://cursor.com/docs/context/mcp))  |
| **VS Code (Copilot)** | yes | yes     | yes     | —       | —       | Tools, resources, prompts, + MCP Apps for interactive UI ([docs](https://code.visualstudio.com/docs/copilot/chat/mcp-servers)) |
| **Windsurf**          | yes | yes     | yes     | —       | —       | stdio/Streamable HTTP/SSE with OAuth; 100-tool cap; MCP Marketplace ([docs](https://docs.windsurf.com/windsurf/cascade/mcp))   |
| **Zed**               | yes | —       | yes     | —       | —       | Handles `notifications/tools/list_changed`; no resources, sampling, elicitation yet ([docs](https://zed.dev/docs/ai/mcp.html)) |
| **Continue**          | yes | unclear | unclear | —       | —       | MCP in *agent mode only* ([docs](https://docs.continue.dev/customize/deep-dives/mcp))                                          |

Sampling remains the least-implemented capability; Cursor explicitly doesn't support it and most IDEs haven't either.

### OpenAI, Google, and cross-vendor convergence

**OpenAI** adopted MCP in phases through 2025:

- **March 26, 2025**: Agents SDK MCP support; commitment to API + ChatGPT desktop
- **May 21, 2025**: MCP in **Responses API** ([docs](https://platform.openai.com/docs/guides/tools-remote-mcp))
- **October 6, 2025** (DevDay): **Apps SDK** launched — ChatGPT apps are literally MCP servers that additionally return embedded UI resources for chat-surface rendering ([developers.openai.com/apps-sdk](https://developers.openai.com/apps-sdk/concepts/mcp-server))

**Google Gemini's** main API docs ([ai.google.dev](https://ai.google.dev/gemini-api/docs)) still do **not** surface MCP as first-class. MCP lives in the **Agent Development Kit (ADK)** at `adk.dev`. No public evidence of the Gemini API natively accepting MCP servers the way the OpenAI Responses API does. Google is a reported founding AAIF member, so deeper integration seems likely but unverified.

**Mistral** shipped MCP support in its agent API May 27, 2025. **LangChain** now wraps MCP servers as tools — an adaptor layer, not a competing protocol.

**Net assessment**: MCP is the closest thing to **HTTP-for-agent-tools** that exists. OpenAI's Apps SDK built on MCP rather than a proprietary scheme is the strongest convergence signal.

### Server publishers, registry, inventory

**Official reference servers** ([github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)) were narrowed to seven educational examples (Everything, Fetch, Filesystem, Git, Memory, Sequential Thinking, Time). Thirteen previously-official third-party integrations (GitHub, GitLab, Slack, Postgres, Brave Search, Google Drive, Puppeteer, Sentry, SQLite, etc.) were **archived** — Anthropic pushed vendors to own their first-party servers.

**Official MCP Registry** ([registry.modelcontextprotocol.io](https://registry.modelcontextprotocol.io/)) — "app store for MCP servers":

- Preview Sep 8, 2025
- API freeze v0.1 Oct 24, 2025
- Maintainers: Adam Jones (Anthropic), Tadas Antanavicius (PulseMCP), Toby Padilla (GitHub), Radoslav Dimitrov (Stacklok), + Block / VS Code / NuGet / Microsoft
- Namespace proof via GitHub OAuth/OIDC, DNS, or HTTP verification → `io.github.username/server` or reverse-domain names
- Deliberately **thin** authoritative root; third-party subregistries (Glama, PulseMCP, Smithery) handle richer discovery

**Third-party directories**:

- **Glama MCP directory** ([glama.ai/mcp/servers](https://glama.ai/mcp/servers)): **21,891 open-source servers** (Apr 21, 2026). 9,748 remote-capable; top categories: Developer Tools (7,725), App Automation (4,057), Search (4,031)
- **PulseMCP** ([pulsemcp.com](https://www.pulsemcp.com/servers)): **12,992 servers** (Apr 2026); runs "THE MCP newsletter"; feeds official registry
- **awesome-mcp-servers** ([github.com/punkpeye/awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers)): 85.2k GitHub stars
- **mcpservers.org**: curated, 11 categories; sponsored listings (Bright Data, Scout, Alpha Vantage); added an Agent Skills directory
- **Smithery.ai**: referenced in a16z's MCP piece but fetch was 403-blocked — adoption numbers unverified

Category inventory (stable for 12+ months): filesystem/Git, databases (Postgres, MySQL, SQLite, Snowflake, BigQuery, ClickHouse), SaaS (Slack, Notion, Linear, Jira, Atlassian, Asana, GitHub, GitLab, Intercom, Stripe, Salesforce, HubSpot), search (Brave, Exa, Perplexity, Tavily), cloud (AWS, Cloudflare, GCP), analytics, browsers/automation (Playwright, Puppeteer, Chrome DevTools), long tail of productivity and niche integrations.

### Strengths and limitations (April 2026)

**Strengths:**

- Cross-vendor standardization achieved
- OAuth 2.1 auth story mature after June + Nov 2025 revisions
- Streamable HTTP transport works behind standard CDN/load-balancer
- Clear governance under LF reduces single-vendor-capture risk

**Limitations:**

- **Token cost is the dominant complaint.** A popular MCP server can consume 55,000+ tokens just advertising tools ([Simon Willison, 2025-10-16](https://simonwillison.net/2025/Oct/16/claude-skills/)). GitHub's official MCP is repeatedly cited as an offender.
- **Security**: the "lethal trifecta" (private-data access + untrusted content + outbound communication; [Simon Willison, 2025-06-16](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/)) has been demonstrated against Microsoft 365 Copilot, GitHub's official MCP, GitLab Duo. Specific incidents:
  - **Notion 3.0 (Sep 19, 2025)**: hidden PDF instructions → data exfiltration
  - **Cowork/Anthropic allowlist bypass (Jan 2026)**: `api.anthropic.com/v1/files` used as exfil endpoint
  - Tim Kellogg's "MCP Colors" framework (late 2025): red (untrusted input) vs blue (critical actions); never mix
- Empirical study of 1,899 servers found 7.2% with general vulns, 5.5% with MCP-specific tool poisoning, 66% with code smells ([arXiv 2506.13538](https://arxiv.org/abs/2506.13538))
- Official registry is young, no ratings/reviews, namespace-squatting risk
- No cross-session or cross-server orchestration in the protocol itself
- Sampling rarely implemented client-side
- Prompt injection via tool descriptions fundamentally unsolvable at protocol level

### Implications for the founder

- **MCP is now the default interop layer for AI tool-calling, not an Anthropic bet.** Exposing Astrolog as an MCP server makes it immediately reachable from Claude, ChatGPT, Copilot, Cursor, Windsurf, Zed, and the Agents SDK without separate integrations.
- **Registry listing is cheap and worth doing.** Publishing under `io.github.<you>.astrolog` or your verified domain gives you official-registry presence.
- **Design with token cost in mind from day one.** If your MCP advertises 40 tools (natal, transit, synastry, progression, solar return, profection, firdaria, etc.), you'll balloon context the way GitHub-MCP did. Use **a small tool surface with rich arguments**.
- **Auth is solved for you.** OAuth 2.1 + dynamic client registration is in-spec — no custom auth design.
- **Plan for the lethal trifecta.** An astrology MCP that accepts untrusted natal data + reads conversation context + emits URLs could be weaponized. Keep output as pure data; avoid emitting executable links.
- **Watch the Apps SEP.** If you ever want to render a chart in-chat, MCP Apps is the path — but it's draft; core server should work without it.

---

## Cluster 2 — Agent skills as a parallel / complementary pattern

### What are agent skills?

An Agent Skill is a folder containing a `SKILL.md` file (YAML frontmatter + Markdown body) plus optional supporting files (reference docs, examples, executable scripts). Frontmatter has a `description` loaded into the agent's tool/skill listing; when the model decides the skill is relevant — or a user types `/<skill-name>` — the full `SKILL.md` body is injected and bundled scripts become runnable via the agent's existing Bash/execution tools ([Claude Code skills docs](https://code.claude.com/docs/en/skills)).

**Timeline:**

- **October 16, 2025**: Anthropic launches Claude Skills ([claude.com/blog/skills](https://claude.com/blog/skills))
- **December 18, 2025**: Org-wide skills management + partner skills directory
- **April 2026**: format is cross-vendor under **Agent Skills** at [agentskills.io](https://agentskills.io), listed as "originally developed by Anthropic, released as an open standard"

`agentskills.io` lists 35+ compatible products: Claude/Claude Code, Gemini CLI, GitHub Copilot, VS Code, OpenAI Codex, Cursor, Amp, Goose (Block), Letta, Roo Code, OpenHands, OpenCode, Laravel Boost, Spring AI, Mistral Vibe, Databricks Genie Code, Snowflake Cortex Code, Kiro, Factory, Junie (JetBrains), and others. **Agent Skills went cross-vendor in ~6 months, following roughly the same trajectory MCP took in its first 6 months.**

### Mechanism vs MCP

|                | **Agent Skills**                                                                        | **MCP servers**                                           |
| -------------- | --------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| **Transport**  | Files on disk (or plugin bundle)                                                        | JSON-RPC over stdio / Streamable HTTP                     |
| **Discovery**  | Agent scans skills directories; descriptions (~few dozen tokens each) always in context | `tools/list` call on server; full tool schemas in context |
| **Invocation** | Model selects → rendered SKILL.md injected as a single message                          | Model selects → JSON-RPC call → response                  |
| **Execution**  | Agent's own runtime (reads files, runs scripts via Bash)                                | Separate process or remote server                         |
| **Auth**       | Filesystem permissions                                                                  | OAuth 2.1, API keys                                       |
| **State**      | Stateless                                                                               | Stateful JSON-RPC session                                 |
| **Token cost** | Minimal until invoked; full body loads on demand                                        | Full tool schemas always in context                       |
| **Network**    | Not required                                                                            | Usually required                                          |

Simon Willison's April 2026 framing: *"Almost everything I might achieve with an MCP can be handled by a CLI tool instead"* — an agent with a shell + markdown playbook is frequently a cheaper substitute for a stateful MCP server.

### OpenAI, Google, and third-party equivalents

**OpenAI:**

- **Custom GPTs** (Nov 2023, pre-MCP): prompt + files + Actions. Closest to skills, but ChatGPT-only.
- **ChatGPT Apps (Apps SDK, Oct 2025)**: MCP-based; fills the "MCP server" slot, not the "skill" slot.
- **OpenAI Codex skills**: adopted the **Agent Skills open standard** ([developers.openai.com/codex/skills](https://developers.openai.com/codex/skills/)). So OpenAI ships both — Apps SDK for MCP surface, Codex skills for Agent Skills surface.

**Google:**

- **Gemini Gems**: closed, consumer-facing custom assistants (2024). Not developer-distributable.
- **Gemini CLI**: ships Agent Skills support ([geminicli.com/docs/cli/skills](https://geminicli.com/docs/cli/skills/)).
- **ADK**: its own framework, not a skill format.

**Third-party**: LangChain has "tool" but no skill/prompt-bundle standard. Spring AI treats Agent Skills as a generic Java primitive ([Jan 13, 2026](https://spring.io/blog/2026/01/13/spring-ai-generic-agent-skills/)).

### When skill vs MCP vs both

**Skill** when:

- Procedural knowledge / playbook (style guide, domain ontology, workflow)
- Tied to code/scripts that run in the agent's own runtime
- Fine to package as portable files
- Cheap when dormant, rare when active
- Deterministic: given inputs X, run `script.py X`

**MCP server** when:

- Live/networked (SaaS API, DB, stream)
- Stateful across calls
- Requires server-managed auth or metered access
- Shared across many users/agents, not shipped per-agent
- Must survive independently of any client's runtime

**Both** is increasingly idiomatic:

1. **Skill wraps an MCP server** — the SKILL.md tells the agent *how to use* a specific MCP server effectively (which tools in what order, how to interpret output). MCP = raw capability; skill = judgment.
2. **MCP server ships alongside a matching skill** — the vendor publishes both: the server for API access + the skill for workflow.
3. **Skill as thin wrapper over a CLI** — per Willison, many MCP servers could just be `skill + bash CLI`.

**Heuristic**: *"If it's a function call, it's MCP. If it's a reasoning pattern, it's a skill. If it's both, ship both."*

### Implications for the founder

- **Ship both a skill and an MCP server for Astrolog — they solve different problems.** MCP = the network-accessible calculation engine. Skill = the astrologer's playbook: "given this chart, interpret Mercury retrograde; here's our house-system convention; here's what VOC Moon means for planning."
- **Your onboarding problem may shrink under this split.** Much of the "needs a video call" friction in AstroPrompt is procedural knowledge that currently has to live in the UI. It could be a skill — loaded when a user asks an astrology question — rather than a product surface.
- **Skill distribution is now cross-vendor.** A single `bad-astrology` or `daily-planner` skill folder targets ~30 agent products via the Agent Skills open standard.
- **Registry + skill listing is your new "app store presence."** Two days of work; multi-vendor distribution; zero marketing spend.
- **If you do the infrastructure pivot, "MCP server + skill pack" is the concrete deliverable.** The classifier is a *skill* that knows when to call your astrology *MCP server*.
- **Watch the token-cost failure mode.** 3–6 carefully-shaped tools (`calculate_chart`, `calculate_transits`, `calculate_synastry`, `get_current_sky_state`) with rich arguments; push interpretive knowledge into the skill layer.
- **Security flag:** birth data is PII. Keep the server pure-calculation, no network egress, no prompt reflection.

---

## Cluster 3 — Tool use and specialized calculation in LLM apps generally

### The broader pattern

Function calling / tool use has moved from experimental add-on to default architecture. Intellectual lineage: **Toolformer** (Schick et al., Meta, Feb 9 2023, [arXiv 2302.04761](https://arxiv.org/abs/2302.04761)); **Gorilla** (Patil et al., Berkeley, May 24 2023, [2305.15334](https://arxiv.org/abs/2305.15334)); **ToolLLM / ToolBench** (Qin et al., Tsinghua, Jul 31 2023, [2307.16789](https://arxiv.org/abs/2307.16789)) scaling to 16,464 RESTful APIs with a neural retriever.

By early 2026 the question has flipped from "can LLMs use tools" to "can they use them without drowning in tokens." **ComplexFuncBench** ([Jan 2025](https://arxiv.org/abs/2501.10132)) documents that even SOTA models stumble on multi-step function calling in 128k contexts. **ToolSandbox** ([Apple, Aug 2024](https://arxiv.org/abs/2408.04682)) extends evaluation to stateful, conversational tool use. The shift is visible in protocols: MCP (Nov 2024); Cloudflare remote MCP (Mar 25, 2025); OpenAI Apps SDK (Oct 2025); MCP donation to AAIF (Dec 9, 2025).

Three mechanisms coexist:

- **Inline function calling** (native APIs) — single-turn JSON tool schemas
- **MCP servers** — out-of-process providers with discovery, resources, prompts. Adoption crossed OpenAPI's GitHub-star trendline by mid-2025 ([latent.space](https://www.latent.space/p/why-mcp-won))
- **Code-execution sandboxes** — agent writes code against tools-as-libraries. Anthropic's "Code execution with MCP" (Nov 4, 2025) was shipped precisely because raw MCP schemas in context were too expensive. HuggingFace **smolagents** (Dec 31, 2024) builds the same idea from the opposite direction — agents write Python, import tools as functions

### Named cases by domain

**Math.** Wolfram's ChatGPT plugin launched Mar 23, 2023 ([writings.stephenwolfram.com](https://writings.stephenwolfram.com/2023/03/chatgpt-gets-its-wolfram-superpowers/)). The current **Wolfram Agent One API** returns "both the LLM-generated response and the computations that inform the response," positioned as a "drop-in endpoint that produces verifiable output." The more common 2026 pattern is in-model code interpreter + SymPy/NumPy, which has largely swallowed pure arithmetic/algebra use cases.

**Finance.** Market data is a heavy MCP category — Lona Trading, aTars MCP, and similar crypto/equity servers show up in the registry. Bloomberg is a founding AAIF member, which signals but does not yet confirm a Bloomberg MCP. Alpha Vantage appears as a sponsor on mcpservers.org.

**Legal. Harvey AI** is the clearest commercial success: founded summer 2022; Series A Apr 2023 ($23M Sequoia); Series G Mar 2026 at **$11B valuation**, $190M 2025 revenue, 60+ AmLaw 100 firms ([Wikipedia](https://en.wikipedia.org/wiki/Harvey_(software))). Harvey does not publicly expose MCP endpoints — it's a closed vertical app. **"Losing MCP / winning product"** case.

**Medical. OpenEvidence** (founded 2022 by Daniel Nadler, ex-Kensho) reached **$12B valuation Jan 2026**, 760,000 registered US physicians, ~18M monthly consultations, 100% on USMLE by 2025 ([Wikipedia](https://en.wikipedia.org/wiki/OpenEvidence)). Like Harvey, closed app, not MCP. **Glass Health** (glass.health) similar. **UpToDate** (Wolters Kluwer) has not publicly released an LLM-accessible endpoint.

**Sports / scientific.** Registry listings for arXiv/PubMed/molecular-sim wrappers exist but none at Harvey/OpenEvidence scale. PulseMCP's top servers by weekly traffic are **Playwright** (2.5M visitors), **DuckDB** (1.7M), **Excel manipulation** (1.6M), **Word** (1.2M), **Context7 docs** (1M) — all general-purpose developer/productivity.

### Wins vs losses — what made the difference

**Wins** (Harvey, OpenEvidence, Wolfram Agent One) share three traits:

1. Proprietary data or calculation engines base models demonstrably get wrong (USMLE-level medicine, jurisdictional case law, arbitrary-precision math)
2. The product wraps calculation in a domain-native UX and workflow, not a raw tool
3. Monetization flows through the vertical (seat-based SaaS, API partner deals), not through an MCP directory

**Losses / stalled** (ChatGPT plugin store, most long-tail MCPs): lose on discovery and latency. Plugins were deprecated because users didn't find them, quality varied, and selection UX was offloaded to the user. Huntley's Aug 22 2025 analysis — GitHub's MCP adding 93 tools and ~55,000 tokens out of ~176,000 usable ([Simon Willison, 2025-08-22](https://simonwillison.net/2025/Aug/22/too-many-mcps/)) — is the practical reason Anthropic shipped Skills (Oct 2025) and "code execution with MCP" (Nov 2025).

### Implications for the founder

- **Specialized calculation is a real, defensible wedge** when base models demonstrably fail at it. Swiss Ephemeris / Astrolog output falls in the same category as Wolfram math or USMLE-level medicine — LLMs hallucinate planetary positions as readily as they hallucinate case law.
- **The winners productize, not protocolize.** Harvey and OpenEvidence didn't win by publishing MCP endpoints; they won by owning the workflow and charging per seat. An astrology MCP is a distribution tactic, not a moat.
- **Token budget is a first-class design constraint.** A naive astrology MCP that dumps 40 tools will be dead on arrival. Design for ~5 high-level tools with progressive disclosure, or ship as a Skill.
- **Code-execution-over-tools is where the frontier is moving.** A Python-importable `astro` library packaged as a Skill (calc binaries bundled) matches where Anthropic, HuggingFace, and Willison converged in late 2025.
- **Follow the verifiability playbook.** Wolfram Agent One's value prop — "returns both the answer and the computation that produced it" — maps perfectly onto astrology: chart data is auditable, LLM narration is not. Separate the two explicitly in output.

---

## Cluster 4 — Discovery mechanisms (the hard problem)

### How clients know which MCP servers / skills exist

As of April 2026, discovery is overwhelmingly **manual**. Across Claude Desktop, Claude Code, Cursor, Windsurf, VS Code, and ChatGPT's Apps SDK: a human enables a server by name in config, authenticates once, those tools are exposed to the model for the session. Within a session the model sees only what the harness pre-loaded. a16z's deep dive lists "server discoverability" and "dynamic discovery" as unresolved ([a16z](https://a16z.com/a-deep-dive-into-mcp-and-the-future-of-ai-tooling/)).

Semi-automatic discovery exists in narrow pockets:

- **Anthropic Skills** (Oct 16, 2025) use progressive disclosure: name+description always in system prompt; `SKILL.md` loads when matched; bundled references load on demand. Still constrained to skills already installed.
- **OpenAI ChatGPT Apps** surfaces apps the user has enabled.
- **Claude Code / Cursor / Goose** let you reference a catalog, but the user picks.

**There is no production system where a base model, mid-conversation, discovers and installs a previously-unknown MCP server.** That's the hard problem.

### Registries, hubs, marketplaces (April 2026)

**Official MCP Registry** ([registry.modelcontextprotocol.io](https://registry.modelcontextprotocol.io/)):

- 6.7k GitHub stars, 745 forks
- API v0.1 frozen Oct 24, 2025 (pre-GA)
- Maintainers from Anthropic, GitHub, PulseMCP, Block, Stacklok, VS Code, NuGet, Microsoft, Last9
- Designed as upstream source-of-truth with namespace verification (GitHub OAuth/OIDC/DNS/HTTP)
- Intentionally small and curated — my fetch of `/v0/servers` showed only 30 surfaced

**Third-party directories** (with retrieved numbers):

- **Glama**: 21,891 servers; $9 / $26 / $80/mo tiers (hosting slots, 3/10/30 servers) ([glama.ai/pricing](https://glama.ai/pricing))
- **PulseMCP**: 12,992 servers
- **awesome-mcp-servers**: 85.2k GitHub stars, 9.3k forks
- **Smithery.ai**: fetch blocked; positioning referenced by a16z but no hard numbers retrievable
- **mcpservers.org**: 11 categories, sponsored listings, added Agent Skills directory
- **OpenAI Apps SDK directory**: in-product, gated, no separate public directory

### DNS / npm / pip analogue?

The Official Registry is structurally closer to **DNS** (small authoritative root, rich resolvers) than **npm** (single dominant index). It's still in preview / API-v0.1 with no data-durability guarantee, and deliberately thin — names, metadata, install info — with subregistries (Glama, PulseMCP, Smithery) handling richer discovery, ranking, hosting.

The **MCP Bridge** paper ([arXiv 2504.08999](https://arxiv.org/abs/2504.08999), Apr 11, 2025) argues for a REST proxy aggregating many MCP servers behind one endpoint — a gateway pattern becoming its own discovery layer.

### Do base models know about specific MCP servers?

**No published evidence** that frontier labs systematically index MCP server docs during pretraining, nor that base models reliably name specific servers from memory in a useful way. The **MCP-Universe benchmark** (Luo et al., Aug 20 2025, [arXiv 2508.14704](https://arxiv.org/abs/2508.14704)) identifies an "unknown-tools challenge": GPT-5 scored **43.72%**, Grok-4 **33.33%**, Claude-4.0-Sonnet **29.44%** when using real MCP servers they hadn't seen.

Indirect evidence that server docs *do* leak into training data: (a) the 1,899-server empirical study ([arXiv 2506.13538](https://arxiv.org/abs/2506.13538)) — that's the population a scraper would find; (b) popular MCPs (GitHub, Playwright) appear in default model recommendations. But turning that into a marketing channel ("publish an arXiv paper, show up in the model") is **not supported by any retrospective study** I could find.

### Academic work on tool selection from large dynamic inventories

Foundations (flagged >12 months):

- **Toolformer** (Feb 2023); **Gorilla** (May 2023); **ToolLLM/ToolBench** (Jul 2023); **API-BLEND** (ACL 2024); **EASYTOOL** (Jan 2024)

Last 18 months on-topic:

- **Toolshed: RAG-Tool Fusion** ([arXiv 2410.14594](https://arxiv.org/abs/2410.14594), Oct 2024) — vector-DB tool index with pre/intra/post-retrieval phases; 46–56% Recall@5 improvements. Clearest template for "semantic tool retrieval from large dynamic inventories."
- **ComplexFuncBench** ([2501.10132](https://arxiv.org/abs/2501.10132), Jan 2025)
- **MCP: Landscape, Security Threats, and Future Research Directions** ([2503.23278](https://arxiv.org/abs/2503.23278), Mar 2025)
- **MCP Safety Audit** ([2504.03767](https://arxiv.org/abs/2504.03767), Apr 2025)
- **MCP Bridge** ([2504.08999](https://arxiv.org/abs/2504.08999), Apr 2025)
- **Agent Interoperability Protocols survey** ([2505.02279](https://arxiv.org/abs/2505.02279), May 2025)
- **RAG-MCP** ([2505.03275](https://arxiv.org/abs/2505.03275), May 6, 2025) — applies RAG to tool selection: semantic retrieval before LLM engagement. **Prompt tokens reduced >50%; tool selection accuracy tripled from 13.62% baseline to 43.13%.** The academic template for "question classifier → specialized calculator."
- **ToolRegistry** ([2507.10593](https://arxiv.org/abs/2507.10593), Jul 2025)
- **MCP-Universe benchmark** ([2508.14704](https://arxiv.org/abs/2508.14704), Aug 2025)
- **Empirical security study of 1,899 servers** ([2506.13538](https://arxiv.org/abs/2506.13538))

Named labs most active: UC Berkeley (Gonzalez / Gorilla), Tsinghua (ToolLLM / ComplexFuncBench), Meta AI (Toolformer), Apple (ToolSandbox), Huazhong U. (MCP landscape / empirical), plus Anthropic's own engineering writing on progressive disclosure.

### Implications for the founder

- **Discovery is still a human act.** Even a beautifully documented Swiss Ephemeris MCP won't be found mid-conversation by any Claude/ChatGPT session. Plan marketing accordingly — discovery is a growth-marketing problem, not a protocol problem.
- **List everywhere; weight the official registry.** Glama, PulseMCP, awesome-mcp-servers, mcpservers.org — but prioritize the **Official MCP Registry** as the LF/AAIF artifact. Get a verified namespace (`io.github.<you>/astroprompt-mcp` or DNS) early; squatting on "astrology" names is non-trivial risk.
- **Ship as a Skill *and* an MCP server.** Skills have lower token overhead, better fit with progressive disclosure, auto-trigger via description matching; MCP gives you Cursor/ChatGPT/Windsurf distribution. Complements.
- **The "unknown-tools" problem is your friend.** Base models use unfamiliar MCPs poorly. That means a thin server won't ride on base-model smarts. Invest heavily in the `SKILL.md` / tool descriptions / canonical examples.
- **Do not count on "publish arXiv, get absorbed into next pretraining run."** No published measurement of that effect for MCP servers. MCP-Universe shows the opposite — models are bad at servers they haven't specifically seen.
- **Design for verifiability.** Wolfram's "return the calculation alongside the narration" is the right template. Auditability is the one thing the professional astrologer audience will grade you on.

---

## Cluster 5 — Monetization patterns

### Is anyone making money on MCP servers?

**Short answer: no widely-reported MCP server is making standalone money directly off tool calls.** Three targeted passes at Smithery, Glama, Product Hunt, and mcp.so could not surface **three paid-at-the-tool-call MCP servers** with published pricing and adoption. Money today flows:

- **Upstream** — existing SaaS (GitHub, Stripe, Notion, Linear, Canva, Box) wraps its paid API behind an MCP server as an acquisition/retention channel
- **Platform layer** — registries and hosts (Glama, Smithery, Cloudflare) charge for hosting / gateway services

Glama's own plans run $9 / $26 / $80 /mo with equivalent AI credits + hosting slots for 3/10/30 servers ([glama.ai/pricing](https://glama.ai/pricing)). Open-source servers are hosted free; paid tier is private/hosted instances.

### Emerging monetization mechanisms

1. **Metered SaaS API behind an MCP wrapper.** Dominant model. **Wolfram** explicitly launched this in **Feb 2026** with an "MCP Service" tier alongside Agent One API and CAG Component APIs, all routed through a Partnerships program (pricing gated to direct sales) ([Stephen Wolfram, Feb 2026](https://writings.stephenwolfram.com/2026/02/making-wolfram-tech-available-as-a-foundation-tool-for-llm-systems/)). Wolfram's existing LLM API has a free tier at 2,000 non-commercial calls/month with paid production pricing.
2. **Subscription-at-the-client, not the server.** Claude (Pro/Max/Team/Enterprise), ChatGPT (Plus/Pro/Team/Enterprise), Cursor bundle MCP access in seat license. Developers get distribution but **no direct revenue cut**.
3. **Marketplace hosting fees.** Glama's tiers; Cloudflare's Workers-based hosting collect on infrastructure, not tool use.
4. **Enterprise licensing.** Keboola, Benchling, Airtable, Asana expose MCP connectors only for paying customers on existing enterprise plans ([claude.com/connectors](https://claude.com/connectors)).
5. **Agent-initiated commerce (nascent).** **Stripe's Agent Toolkit** (Nov 14, 2024) lets agents create Payment Links, run metered billing, scaffold test transactions inside OpenAI Agents SDK, Vercel AI SDK, LangChain, CrewAI ([Stripe Dev Blog](https://stripe.dev/blog/adding-payments-to-your-agentic-workflows)). Most plausible rail for future pay-per-tool-call MCP servers — but **no named MCP server billing end-users per call through it as of April 2026**.

### Anthropic / OpenAI / Google revenue-share (April 2026)

- **Anthropic**: November 2024 MCP announcement contains no revenue-share provisions; MCP is pitched as open standard. Claude Connectors directory is curated but third-party-maintained; providers set their own terms via OAuth. Claude Skills (Oct 16, 2025) ship through a directory with partners like Box, Canva, Notion. **No published revenue share for skill/connector authors as of April 2026.** The Claude Plugins directory shows high install counts (Frontend Design 564,908, Superpowers 476,245, Context7 268,967 — [claude.com/plugins](https://claude.com/plugins)) but no monetization layer.
- **OpenAI**: Apps SDK launched DevDay Oct 2025, MCP-based. "Monetize your app" section in the docs; specific revenue-share percentages **not published** on the public overview page as of April 2026. Submissions go through review to public ChatGPT apps store; Codex distribution "coming soon."
- **Google**: Adopted MCP April 2025; no public marketplace/revenue-share program for MCP extensions verifiable as of April 2026.

**Net: none of the three major vendors has published a revenue-share % for third-party MCP server / skill / connector authors as of April 2026.** Comp = GPT Store (Jan 2024, 3M+ GPTs) — revenue share was promised for US builders but never broadly materialized.

### Payment rails and auth patterns

- **OAuth 2.1** is the de facto standard for remote MCP servers. Cloudflare's `workers-oauth-provider` (Apr 2025) is the reference ([Cloudflare blog](https://blog.cloudflare.com/remote-model-context-protocol-servers-mcp/)). WorkOS has built dedicated MCP auth flows.
- **API keys** common for developer-to-developer MCP servers, especially stdio/local.
- **Stripe for payments.** Agent Toolkit (Nov 2024) recommends restricted keys (`rk_*`): "limit your agent's access to only the functionality it requires, especially in live mode" ([docs.stripe.com/agents](https://docs.stripe.com/agents)).
- **Pay-per-tool-call** technically possible (Stripe metered + MCP tool response hook) but **no publicly documented production MCP server** using this model as of April 2026.
- **Durable state.** Cloudflare's McpAgent class uses Durable Objects with SQL for persistent per-user sessions — enables stateful billing, carts, per-user rate limits.

### Precedents & cautionary tales

- **Apple App Store**: 30% / 15% fees; sets platform-owner expectation ceiling
- **Chrome extensions**: no direct revenue share — monetization is developer's problem; platform can delist unilaterally
- **ChatGPT plugins**: launched Mar 2023; superseded by GPTs (Nov 2023) and GPT Store (Jan 2024, "more than 3 million GPTs"). Widely reported to be wound down April 2024 (primary OpenAI article was 403-blocked this session — corroborated via Wikipedia timeline). Replacement arc: plugins → GPTs (Nov 2023) → Apps SDK on MCP (Oct 2025). **OpenAI has burned one developer ecosystem already** — anyone building on Apps SDK today should assume the platform may pivot again.
- **Discord bot directories / Zapier apps / Slack app directory**: pattern across all — the marketplace is a discovery surface, not a payment surface. **This is the likely default outcome for MCP as well.**

### Implications for the founder

- **Do not count on a platform revenue share.** Build assuming you run your own billing (Stripe Agent Toolkit + OAuth).
- **The SaaS-behind-an-MCP-wrapper pattern is the only proven monetization path.** Direct analog to Wolfram's Feb 2026 MCP Service tier. Price the underlying compute, not the MCP interface.
- **Platforms pivot.** OpenAI killed plugins ~13 months after launch. Keep your MCP server LLM-agnostic (Cloudflare Workers, Fly, own VPS) so you survive the next pivot.
- **Distribution is the prize, not revenue share.** Claude Plugins directory shows 500K+ installs for top entries with zero monetization. If AstroPrompt's MCP gets that reach, small paid conversion on Astrolog compute is meaningful.
- **Pay-per-tool-call is a whitespace.** No one has shipped it production-scale for MCP as of April 2026. An astrology-calculation MCP with Stripe-metered billing per chart computation would be an early mover — but probably small market until LLM agents become primary consumers.

---

## Cluster 6 — Comparable domain-specific plays

### Specialized calculation / knowledge MCP servers by vertical

- **Scientific / math — Wolfram.** Clearest play in the market. Feb 2026 launch as "foundation tool for LLM systems" with three deployment modes: **MCP Service** (web API for MCP-compatible hosts), **Agent One API** (drop-in LLM+tool replacement), **CAG Component APIs** (Computation-Augmented Generation) ([Stephen Wolfram, Feb 2026](https://writings.stephenwolfram.com/2026/02/making-wolfram-tech-available-as-a-foundation-tool-for-llm-systems/)). Position: LLMs lack "precise knowledge" and "deep computation" — 40 years of Wolfram Language fills the gap.
- **Finance.** Stripe Agent Toolkit is the anchor for payments-as-a-tool. Broader fintech MCP landscape (Plaid, Finch, Ramp) wraps existing APIs; no differentiated MCP-native finance calculators verifiable.
- **Data / analytics.** Keboola MCP Server — "Build production-grade data pipelines with just a prompt" ([Product Hunt](https://www.producthunt.com/search?q=MCP%20server)). Monetization: upstream Keboola subscription.
- **Code.** GitHub MCP Server (official), Context7 (live docs lookup, 268,967 installs via Claude Plugins), Sourcegraph Cody MCP integration.
- **Security.** MCPSafetyScanner (research, not commercial).
- **Legal / medical.** No named MCP servers with public pricing surfaced in these verticals — likely exist but gated to enterprise channels.
- **Astrology / tarot / fitness / nutrition.** **No named MCP server offerings as of April 2026.** Genuine whitespace — but also a signal the market may not yet be big enough for direct monetization.

### Two-layer "question classifier + specialized calculator" architectures

This is exactly the architecture Wolfram described in March 2023: "ChatGPT is formulating a query for Wolfram|Alpha—then sending it to Wolfram|Alpha for computation, and then 'deciding what to say' based on reading the results it got back" ([Stephen Wolfram, Mar 2023](https://writings.stephenwolfram.com/2023/03/chatgpt-gets-its-wolfram-superpowers/)).

Formalized in recent research:

- **RAG-MCP** ([arXiv 2505.03275](https://arxiv.org/abs/2505.03275), May 6, 2025). Explicitly applies RAG to tool selection: "semantic retrieval to identify relevant tools before engaging the LLM." **Prompt tokens reduced >50%; tool selection accuracy tripled from 13.62% baseline to 43.13%.** The academic version of "question classifier → specialized calculator."
- **ToolRegistry** ([arXiv 2507.10593](https://arxiv.org/abs/2507.10593), Jul 2025). Protocol-agnostic tool management with MCP + OpenAPI adapters and semantic routing. Claims 60–80% integration-code reduction, 3.1x concurrent execution speedup.
- **MCIP** ([arXiv 2505.14590](https://arxiv.org/abs/2505.14590), May 2025, EMNLP 2025). Safety-layer version of the classifier pattern.
- **a16z MCP thesis** ([a16z](https://a16z.com/a-deep-dive-into-mcp-and-the-future-of-ai-tooling/)) names discoverability, gateways, execution routing as critical investable layers.
- **Wolfram Agent One API** (Feb 2026) — productized two-layer system.
- **Strata** (Product Hunt) — "One MCP server for AI agents to handle thousands of tools" — gateway/routing play; pricing/traction not verified.

**Net: yes, the classifier + specialized calculator architecture is a named, funded, and published pattern.** Wolfram is the productized comp; RAG-MCP is the academic blueprint; a16z has publicly stated it's investable.

### Astrology / spirituality AI specifically

- **Co-Star**: $20M+ raised across pre-seed, seed, $15M Series A (April 2021). 20M+ downloads. AI-powered from the start — maps "human-written snippets of text to planetary movements." Shipped "The Void" (in-app AI Q&A) July 2023 ([Wikipedia](https://en.wikipedia.org/wiki/Co%E2%80%93Star)). **No B2B API or infrastructure product publicly**, based on verifiable record. *(Series A 2021 — >12 months; no more recent raise reported.)*
- **Sanctuary**: psychic reading marketplace, $4.99 intro / up to $28.99/min ([sanctuaryworld.co](https://sanctuaryworld.co)). **No API, no B2B, no LLM-native offering.**
- **Nebula (asknebula.com)**: psychic reading platform, self-reported 70M users, 1,500+ psychics, 4.7 stars. Per-minute pricing $0.47–$6.99/min. **No AI infrastructure.**
- **The Pattern**: iOS/Android astrology + dating app. No API or B2B visible.
- **Chani**: subscription app + physical products + education. Generated >$2.5M for survivors of gender-based violence via FreeFrom partnership. **No API / B2B / infrastructure.**
- **Kasamba**: "Guided over 3 million people since 1999," $0.99–$39.99/min. Marketplace only.
- **Astrodienst (astro.com)**: ~9M monthly visitors. Operates **Swiss Ephemeris** — the calculation backbone for most astrology apps (including AstroPrompt via Astrolog). **Has not launched an LLM-native MCP wrapper.**
- **Katie Bohinc / Astroloji / Astro Poets**: no current startup or B2B API verifiable in this session.

Across all seven astrology brands: **none have moved from consumer app to a published API / MCP / B2B product.** The underlying calculation stack (Swiss Ephemeris, Astrolog) is already open-source or free-for-use — the interpretive content is where each brand differentiates, and none has chosen to commoditize it via API. **This is a real signal: the astrology market has matured on B2C; the B2B / MCP play is uncontested but unvalidated.**

### Implications for the founder

- **Wolfram is your comp, not Co-Star.** The Feb 2026 MCP Service launch is the closest template for what an astrology-calculation MCP would look like. Read the Wolfram rollout closely; copy the three-tier structure (MCP Service / Agent One API / Component APIs).
- **Zero astrology-specific competition on the infrastructure side.** Genuine whitespace — but also means you're validating the market yourself.
- **The two-layer architecture is vindicated.** RAG-MCP and a16z both name it as important. Your classifier-plus-Astrolog-backend idea is on-trend, not contrarian.
- **Consumer-app astrology remains the large revenue pool.** Nebula's 70M users, Kasamba's 3M since 1999 — consumer per-minute/subscription models monetize better at scale than current MCP infrastructure does.
- **Pricing will need to be partner-sales initially.** Wolfram kept MCP Service pricing behind a partner-program email. At solo-dev scale, start with 3–5 hand-picked LLM/agent-platform partners before building billing infrastructure.
- **Do not give up the B2C surface.** Astrology's revenue is in B2C engagement. An MCP server is a distribution and credibility play — it's how you get Claude / ChatGPT / Perplexity to cite your calculations. The subscription or per-reading billing still lives on your own surface.

---

## Cluster 7 — Investment landscape

### Venture firms and partners writing the checks

**Andreessen Horowitz (a16z).** Infrastructure practice led by **Martin Casado** — board seats on Cursor, Kong, Netlify, Material Security. Public writing 2025–2026 emphasizes "how foundation models transition toward agentic systems" ([a16z](https://a16z.com/author/martin-casado/)). **a16z Speedrun** has deployed "over $180M across 150+ startups" since 2023, SR007 cohort July–Oct 2026 in SF ([a16z Speedrun](https://a16z.com/speedrun/)) — the firm's YC-shaped entry point for AI-native founders.

**Sequoia.** Relevant 2024–2025 agent-infra bets:

- **Agency** — "AI agent for customer success," 2024, partners Pat Grady + Julien Bek
- **Airtop** — intelligent browser automation for AI agents
- **Auctor** — 2025 early-stage, partner Julien Bek; "AI-native system of action for software implementation"
- **Blockit AI** — 2024 pre-seed/seed, partner Pat Grady; AI scheduling agent
- **Anthropic** — growth-stage 2026 follow-on, partners Sonya Huang, James Flynn, Lauren Reeder ([Sequoia portfolio](https://www.sequoiacap.com/our-companies/))

**Index Ventures.** **Wonderful** ($150M Series B, "hyper-local enterprise AI platform"); **Parallel** ($20M, AI agents for hospital workflow) ([Index perspectives](https://www.indexventures.com/perspectives)). 2026 framing: companies moving "from assistive AI to authoritative systems" — the most explicit agent-economy thesis of the major multi-stage firms.

**Greylock.** Anchored by Saam Motamedi, Reid Hoffman, Seth Rosenberg. AI is a core sector ([greylock.com/greymatter](https://greylock.com/greymatter/)); specific MCP-branded deals not surfaced in retrievable content.

**Conviction (Sarah Guo).** $1–25M checks. Partners: Sarah Guo, Mike Vernal, Pranav Reddy, Bella Garcia-Camargo. Agent-infra portfolio: **Mistral, Cognition, Harvey, Baseten, Cartesia, Sierra** ([conviction.com](https://www.conviction.com/)). Sierra is the canonical "agent platform" bet of 2024–2025. Guo's **No Priors** podcast is the single most-cited source of investor narrative.

**General Catalyst.** 2025–2026 flags include Bolna, Stilla, Accrual under "Applied AI & Transformation"; pushing "agentic commerce" ([GC perspectives](https://www.generalcatalyst.com/perspectives)). CEO Hemant Taneja #8 on Forbes Midas 2026.

**Anthropic / Anthology.** `anthology-fund` page returned 404 Apr 2026 — rolled into or superseded by the **Claude Partner Network** (Mar 12, 2026): **$100M** program covering training, sales enablement, co-marketing, 5x expansion of partner-facing team ([Anthropic, Mar 12 2026](https://www.anthropic.com/news/claude-partner-network)). **Notably does not mention revenue share with MCP server developers** — significant for platform-risk analysis.

**OpenAI Startup Fund.** Primary fetches 403. Through secondary reporting: less about seeding MCP startups, more about enabling Apps-in-ChatGPT partners. *(Under-sourced — treat thinly.)*

**Y Combinator.** Spring 2026 RFS explicit on the agent thesis:

- **"AI-Native Hedge Funds"**: "swarms of agents doing what hedge fund traders do now"
- **"Cursor for Product Managers"**: "As agents increasingly take the first pass at implementation, the way we define and communicate 'what to build' needs to change"
- **"Make LLMs Easy to Train"** — infra gaps in training APIs and ML dev environments ([YC RFS](https://www.ycombinator.com/rfs))

Specific W25/S25/W26 company enumeration could not be reliably produced from primary sources this session — the batch-concentration claim is thesis-level, not company-level, in this briefing.

### Notable rounds and deals

- **Runlayer** (MCP agent security): **$11M, November 2025**; Khosla Ventures' Keith Rabois + Felicis leading; "support from eight unicorn companies" ([TechCrunch MCP tag](https://techcrunch.com/tag/mcp/)). **Cleanest example of an MCP-first company raising institutional capital.**
- **Anthropic**: **$5B from Amazon** closed Mar–Apr 2026 with $100B cloud-spend commitment ([TechCrunch April 2026 archive](https://techcrunch.com/2026/04/))
- **Cursor**: in talks to raise at **$50B valuation** with $2B+ round
- **Factory**: hit **$1.5B valuation**
- **Wonderful**: $150M Series B (Index)
- **Parallel**: $20M (Index)
- **No major platform has acquired an MCP directory as of April 2026.** Smithery, Glama, similar registries remain independent. Governance consolidation went the other way — Anthropic donated MCP to the AAIF / Linux Foundation in Dec 2025.

### Public investor narratives

- **Ben Thompson (Stratechery)**: single most influential narrator of the agent-economy thesis. Argues agents are a "transformative third paradigm" after ChatGPT and o1; "model commoditization" is overstated because agents require integrated model-and-harness pairings. Cites Microsoft / Anthropic Copilot partnership as evidence "model-agnostic strategies" have been abandoned for agentic products ([Stratechery](https://stratechery.com/)).
- **Simon Willison**: most prolific technical chronicler, 25+ MCP posts Nov 2024 through Dec 2025. Load-bearing themes: (1) lethal trifecta prompt-injection risk; (2) token cost (GitHub MCP ~55k tokens / 30% context); (3) Dec 2025 LF handoff ([simonwillison.net MCP tag](https://simonwillison.net/tags/model-context-protocol/)).
- **Matt Rickard**: go-to commentator on agent constraint / spec-driven development. "The Spec Layer" Mar 31, 2026 ([mattrickard.com/the-spec-layer](https://mattrickard.com/the-spec-layer)).
- **Nathan Lambert (Interconnects)**: Mar–Apr 2026 covers agents, open-model viability, "model consortia" as infrastructure ([Interconnects archive](https://www.interconnects.ai/archive)).
- **Sarah Guo (Conviction / No Priors)**: "we are extremely early in the translation of powerful AI models to powerful products."
- **Martin Casado (a16z)**: emphasizes "open standards" and the transition from LLMs to agentic full-stack systems.
- **Pat Grady (Sequoia)**: deploying capital into agents (Agency, Blockit AI); less public writing than Huang.
- **Dan Shipper, Elad Gil, Shawn Wang (swyx)**: primary-source fetches partial/blocked. Latent Space confirms swyx covering "Cloud Agents" as next paradigm ([Latent Space archive](https://www.latent.space/archive)). *(Flagged — these thought-leader citations are under-sourced here.)*

### Hype-cycle read (April 2026)

**"Past peak narrative enthusiasm, entering reality-testing, but still rising in capital deployment."** Evidence:

- **Still rising in capital**: Anthropic $5B (Amazon), Cursor $50B, Factory $1.5B, OpenAI's $122B round with $3B from retail investors — all reported Mar–Apr 2026. Peak-bubble numbers.
- **Peaking in narrative**: Ben Thompson's April 2026 framing of agents as a done deal for enterprise ROI implies the thesis is now consensus rather than contrarian — historically a late-cycle signal.
- **Early disillusionment on MCP specifically**: Willison documents practitioners preferring CLI utilities over MCP because of token overhead. Anthropic has proposed converting MCP tools to "code functions" — an admission that original design is inefficient.
- **Early failures**: Yupp AI shut down late March 2026 after raising $33M from a16z crypto's Chris Dixon ([TechCrunch March 2026](https://techcrunch.com/2026/03/)). First high-profile agent-adjacent shutdown of the cycle.

**Net: MCP-specifically is past the initial speculative peak (Nov 2024 → mid-2025) into the "does this work under real load?" phase. Agent infrastructure *generally* is still in peak capital deployment.** These two curves are not the same.

---

## Platform-risk appendix

### What happens to third-party MCP servers if Anthropic or OpenAI ships first-party versions?

**The third-party product does not disappear, but its growth ceiling collapses and its exit paths shrink to acqui-hire.** Platforms ship first-party versions when:

1. The category becomes load-bearing for the platform's own UX
2. The category is narrow enough that one good implementation wins
3. Platform owner judges the LTV of owning the category is higher than goodwill cost of alienating devs

For **specialized, vertically-narrow MCP servers** (e.g., Swiss-Ephemeris-backed astrology), risk is *lower* than for horizontal categories (a better GitHub MCP) — the platform has no incentive to integrate a niche vertical. But "lower" is not "zero," and revenue-share silence from the Partner Network is a warning.

### Precedents

1. **ChatGPT Plugins sunset (April 2024).** Plugins launched March 2023; GPTs Nov 2023; GPT Store Jan 2024 ("more than 3 million GPTs"); plugins effectively deprecated April 2024 ([Wikipedia — ChatGPT](https://en.wikipedia.org/wiki/ChatGPT)). Progression — plugins → customization → marketplace → ChatGPT Apps (Dec 2025) — shows the platform treats dev ecosystem as **a feature-discovery funnel, not a durable business layer**.
2. **GPT Store trajectory.** From "3M GPTs" at Jan 2024 launch to a rebuilt "ChatGPT Apps" using @mention invocation Dec 2025. GPT Store did not produce durable businesses; platform re-architected around a new invocation model; first-party OpenAI offerings (search, Canvas, code interpreter, memory) absorbed many third-party use cases.
3. **Apple Sherlocking.** Term from Apple's Sherlock 3 (Mac OS X 10.2, 2002) absorbing Karelia Software's **Watson** ([Wikipedia](https://en.wikipedia.org/wiki/Sherlock_(software))). Sherlock itself later subsumed by Spotlight (10.4, 2005), removed in Leopard (10.5, 2007) — platform killed both third-party product *and* its own replacement. Recent: Widgets, Reeder-style features into Apple News, Journal app vs Day One, password manager vs 1Password.
4. **Slack third-party app developers.** Slack has repeatedly changed API pricing, deprecated features apps relied on, tightened data policies post-Salesforce. Pattern: build thriving ecosystem → progressively capture more of the value.
5. **Chrome Web Store.** Manifest V3 deprecation of V2 (2024–2025) killed/hobbled extension categories — most visibly ad blockers. Platform framed it as security; effect made whole categories structurally less powerful.

### Anthropic / OpenAI public commitments

- **Anthropic's Claude Partner Network** ($100M, Mar 12 2026) pledges partner enablement but **not revenue share for third-party MCP servers.** Enterprise-services oriented, not developer-platform oriented.
- **MCP governance handoff to LF (Dec 2025)** is genuinely pro-developer: the protocol is outside Anthropic's unilateral control. But an open protocol does not protect individual MCP **businesses** from absorption.
- **No anti-copycat policy** from Anthropic or OpenAI verifiable as of April 2026.

### Recent platform-absorption examples (2024–2026)

- **ChatGPT Apps (Dec 2025)** replaced the GPT Store model, obsoleting ~2 years of GPT-builder investment
- **Claude Design (Apr 17, 2026)** — "collaborative creation of visual work including designs and prototypes" — absorbs functionality some prompt-to-mockup third-party tools and design MCP servers were building
- **Claude Opus 4.7 (Apr 16, 2026)** — "stronger performance across coding, agents, vision, and multi-step tasks" — each capability bump hollows out wrapper products relying on older model behavior
- **OpenAI ChatGPT Apps @mention invocation (Dec 2025)** — platform becomes the store; third-party apps compete for @mention real estate, not their own distribution

---

## Cluster 8 — Strategic implications for a niche calculation/classification provider

### The five options evaluated

The research validates — and in places qualifies — the five options named in the brief. Summary first, then the comparison table.

#### Option A — Open-source + paper route

*Publish MCP server open-source; publish arXiv paper; rely on training-data osmosis and community discovery.*

**Honest read**: the "training-data osmosis" half of this is **not supported by the evidence**. MCP-Universe data shows models score 29–44% on unfamiliar servers even with a full spec sheet in front of them. No retrospective study shows MCP server docs measurably surfacing in later pretraining runs. Community discovery via GitHub/awesome-mcp-servers is real but slow — it's a minor tailwind, not a strategy.

**Still worth doing as a *component* of any strategy** — open-source lowers trust friction, papers build credibility with the investor / academic audience, and the Astrolog wrapper should be open anyway since Astrolog itself is GPL. Just don't count on it as the distribution mechanism.

#### Option B — Marketplace route

*Publish to existing MCP directories / registries; monetize via metered API behind the server.*

**Honest read**: the marketplaces are discovery surfaces, not payment surfaces — consistent with Zapier, Slack, Chrome, and the GPT Store pattern. No platform has published a revenue-share for MCP developers as of April 2026. The directories (Glama 22k, PulseMCP 13k, awesome-mcp-servers 85k stars) are useful for SEO and developer discovery, but end-user distribution still depends on the user knowing to install your server.

**Time-to-evidence is fast (weeks) and cost is low**, which makes this a good *first move* but a weak *strategy*. The real question this path answers: "does anyone install my astrology MCP at all, and do they come back?" That's worth knowing before more expensive bets.

#### Option C — Skill-first route

*Package as Claude skill / Codex skill / Gemini CLI skill via Agent Skills open standard.*

**Honest read**: the strongest distribution bet of the five for a procedural-knowledge-heavy product. Skills auto-load by description matching, carry near-zero token cost until invoked, and with the Agent Skills open standard now covering ~30 agent products, a single skill folder reaches most of the audience.

**Skill-first fits your onboarding problem.** A lot of AstroPrompt's current video-call friction is procedural knowledge: "here's how to read a chart, here's what VOC Moon means for your plan, here's the house-system convention we use." That content belongs in a skill, not a UI.

The weakness: **skills without a backing calculation engine are Wikipedia articles.** The skill works only because it can call either (a) an MCP server you run or (b) a Python CLI / library you bundle. So "skill-first" is really "skill-wrapping-either-MCP-or-CLI."

#### Option D — Two-layer route

*Question-classifier MCP server (layer 1) routes astrology-shaped questions to specialized calculator MCP servers (layer 2).*

**Honest read**: this is the pattern with the most *intellectual tailwind* and the least *current evidence*. RAG-MCP (May 2025) demonstrates a 3x accuracy improvement and >50% token reduction for semantic tool selection — academically validated. a16z explicitly calls out this layer as investable. Wolfram Agent One (Feb 2026) is the first productized version.

But: **building a question classifier is a lot more work than wrapping Astrolog.** The classifier has to (a) detect when a question is astrology-shaped, (b) route to the right tool within your calculator, (c) compose the result back into natural language. You are now building a small agent, not a tool. And the classifier's market is much larger than astrology — whoever wins horizontal classification/routing (Strata, Smithery gateway, ToolRegistry) will subsume you if you try to own both layers.

The pragmatic read: **focus your effort on being a high-quality layer-2 calculator** that horizontal classifiers can route *to*. Whoever builds the classifier will look for best-in-class specialized calculators; be that.

#### Option E — Gate + partnership route

*Keep API proprietary; pursue direct partnerships with AI platform vendors or prosumer app vendors.*

**Honest read**: this is what Wolfram actually did in Feb 2026 — pricing behind `partner-program@wolfram.com`, three deployment modes, no public per-call pricing. For a solo founder with no sales team, this is harder than it sounds: partnership sales cycles run 3–12 months, incumbents set the pace, and your leverage is weak unless you have either (a) irreplaceable data, (b) a captive consumer audience large enough to matter, or (c) a name brand.

AstroPrompt has none of (a)–(c) yet. The calculations are replicable by anyone with Astrolog or Swiss Ephemeris. A partnership play makes sense **after** you've proven demand through cheaper channels (B, C), not before.

### Strategic options comparison

| Option                                     | Preconditions                                                                                          | Time-to-evidence                                                 | Capital required                                                                                    | Incumbent risk                                                                                                                               | Upside if it works                                                                                                          | Recommended next action                                                                                                                                                                                                                     |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **A. Open-source + paper**                 | Willingness to GPL the MCP wrapper; drafting bandwidth for arXiv                                       | 6–12 months to measurable traction (inbound links, GitHub stars) | Low — solo-dev time only                                                                            | Low — no category for anyone to copycat                                                                                                      | Modest — credibility and slow organic discovery                                                                             | Publish the MCP server to GitHub under GPL (Astrolog is already GPL); draft short paper, submit to arXiv cs.AI or cs.CL; use as **supporting evidence**, not primary strategy                                                               |
| **B. Marketplace**                         | A working MCP server with small tool surface (3–6 tools); docs and examples                            | 2–8 weeks to install counts, return visits, error logs           | Very low — hosting on Cloudflare Workers ~$0–25/mo to start                                         | Low short-term; medium if Anthropic/OpenAI later build an astrology-adjacent first-party (unlikely)                                          | Weak direct monetization; strong **validation signal**                                                                      | Ship to Official MCP Registry + Glama + PulseMCP + awesome-mcp-servers within 30 days; instrument usage; treat as market test, not revenue engine                                                                                           |
| **C. Skill-first**                         | A crisp, useful SKILL.md + bundled calculator (CLI or MCP); clear audience segment                     | 1–3 months to skill-directory install counts and usage telemetry | Low — solo-dev time + hosting for any backing API                                                   | Low — skills are portable across ~30 agent products; Agent Skills is an open standard                                                        | High — the *only* channel that carries procedural astrology knowledge into any LLM agent context cheaply                    | Build a `bad-astrology` or `daily-planner-astro` skill folder with the SKILL.md, reference docs, a bundled Python CLI over Astrolog; ship to the Agent Skills directory, Claude's partner skills directory, Codex skills, Gemini CLI skills |
| **D. Two-layer (classifier + calculator)** | Willingness to build *and maintain* a question classifier; comfort with RAG/semantic-retrieval tooling | 6–12 months to working system; 12–24 months to adoption          | Medium — solo-dev year of work minimum; possibly needs hosted vector DB, embedding costs            | High — horizontal routing is exactly what Strata, Smithery, ToolRegistry, and eventual Anthropic/OpenAI platform features will try to absorb | High if you own the classifier for spirituality/wellness broadly; near-zero if horizontal players commoditize routing first | **Do not build the classifier yourself.** Build a best-in-class layer-2 calculator (the Astrolog MCP + skill) with clear intent metadata so horizontal classifiers can route to it. Revisit in 12 months.                                   |
| **E. Gate + partnership**                  | Name-brand, captive audience, or irreplaceable data; 3–12 month sales cycles                           | 6–18 months to first signed partner; 12–24 months to revenue     | Medium-high — your time and the opportunity cost of not shipping; possibly travel, contracts, legal | Low — you control access; but platform shifts can kill the partner deal                                                                      | High if one partner integrates deeply; low-to-zero if they don't                                                            | Defer until B and C produce traction signals. Then pursue 1–2 hand-picked partners (possibly an astrology software publisher, or an LLM platform looking for verified-calculation exemplars for their "accurate-domain" story)              |

### Recommended sequencing

The research collapses to a clear sequencing argument for a solo founder:

1. **Weeks 1–4**: Ship an open-source MCP server (Option A + B combined). 3–6 tools, Cloudflare Workers, Stripe-restricted-keys on a free tier with metered paid tier. List on Official Registry, Glama, PulseMCP, awesome-mcp-servers. Cost: 1 month solo-dev.
2. **Weeks 4–12**: Build and ship a Skill pack (Option C) that wraps the MCP server *and* bundles a CLI over Astrolog. Target the Agent Skills directory, Claude partner skills, Codex skills, Gemini CLI skills. Cost: 2 months solo-dev.
3. **Month 3–6**: Read usage telemetry. If the MCP + skill get meaningful inbound (thousands of invocations, not hundreds), pursue 1–2 hand-picked partnerships (Option E) — LLM vendor looking for calc-exemplar, or an astrology publisher white-labeling. If inbound is thin, abandon the infrastructure pivot and pull the procedural-knowledge learnings back into AstroPrompt's consumer UX.
4. **Month 6–12**: Decide whether horizontal classifier routing (Option D) has a winner. If one emerges (Strata, Smithery gateway, ToolRegistry, or an Anthropic/OpenAI first-party feature), invest in being the best-addressed layer-2 calculator for spirituality/wellness routing. Do not build the classifier yourself.

The architecture bet from your CLAUDE.md framing — "question classifier + specialized MCP servers" — is on the right side of where the ecosystem is going. The research says: **let someone else build the classifier; own the specialized calculator; carry the procedural knowledge in a skill; keep the consumer app as your revenue surface.**

---

## What I don't know

Questions the research could not resolve from publicly available information as of April 2026:

1. **Actual paid-MCP-server revenue data.** Three targeted searches could not name three paid-at-the-tool-call MCP servers with published pricing and adoption. Either the market is thinner than the press suggests, or the numbers are private. Worth a direct Stripe / Cloudflare ask if a contact is available.
2. **Smithery's adoption and business model.** Primary page fetch was 403-blocked throughout research; referenced by a16z but specific server count, MAU, and funding details were not retrievable.
3. **OpenAI Apps SDK revenue-share percentage.** The docs reference "monetize your app" without specifying terms. Requires a direct OpenAI partner conversation.
4. **Whether arXiv / GitHub publication measurably improves base-model recall.** Plausible in principle, but no retrospective study found. Remains testimonial.
5. **Linux Foundation Agentic AI Foundation founding-member roster.** Corroborated via Simon Willison coverage; primary LF press release was not retrievable this session.
6. **Specific YC W25/S25/W26 MCP-first company enumeration.** YC directory did not surface enough batch-level detail; the batch-concentration claim is thesis-level, not company-level.
7. **Google Gemini API's MCP posture.** Google is reported AAIF founding member; `ai.google.dev` doesn't surface MCP as first-class; ADK has it but depth is unclear. Worth direct ADK docs re-check.
8. **Benchmark and OpenAI Startup Fund agent-infra portfolios.** Minimal primary-source transparency; would require Crunchbase / Pitchbook data.
9. **Whether any astrology brand (Co-Star, Chani, Nebula, etc.) has a private B2B API program.** Negative finding in public sources — may exist privately. Worth asking directly if the founder has warm contacts.
10. **Real-world usage data for Claude Skills in the wild.** Install counts are visible ([claude.com/plugins](https://claude.com/plugins)) but invocation frequency, retention, and monetization conversion are not public.
11. **The specific April 9, 2024 OpenAI ChatGPT plugin sunset primary article.** `help.openai.com/en/articles/8988022` returned 403/404. Corroborated via Wikipedia timeline but the primary artifact is unreachable.
12. **Which CDNs / infrastructure partners are seeing MCP traffic at scale.** Cloudflare has published blog posts; AWS and Fly have not publicly. Would be useful for sizing the actual production MCP footprint.

---

*End of briefing.*
