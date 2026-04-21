---
- Version: 1
- Date: 2026-04-21
- Author: Codex
- Depends on: prompt-mcp-ecosystem-research-plan.md
---

# MCP Ecosystem, Agent Skills, and Tool-Use Infrastructure: Strategic Briefing

**Prepared:** April 21, 2026  
**Audience:** solo founder-developer evaluating whether a specialized calculation/data service should become an MCP server, agent skill, API product, or some combination.  
**Scope note:** this report prioritizes evidence from roughly April 2025 to April 2026. Older sources are explicitly flagged where they remain relevant because the best primary source is older than 12 months.

## Executive Summary

- MCP has moved from Anthropic-originated experiment to cross-platform agent infrastructure. Anthropic donated MCP to the Linux Foundation's Agentic AI Foundation on December 9, 2025, and claimed more than 10,000 active public MCP servers, adoption by ChatGPT, Cursor, Gemini, Microsoft Copilot, and VS Code, and 97M+ monthly SDK downloads across Python and TypeScript ([Anthropic, Dec. 9, 2025](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation?content=Dec2024EOYShips&medium=email&messageTypeId=140367&source=i_email)).
- The current MCP specification baseline is the stable `2025-11-25` revision; major recent changes include stronger authorization discovery, tool/resource/prompt icons, elicitation improvements, tool calling in sampling, OAuth client metadata, experimental durable tasks, and formalized governance/working groups ([MCP changelog, Nov. 25, 2025](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/docs/specification/2025-11-25/changelog.mdx)).
- Discovery remains the hard problem. The official MCP Registry launched in preview on September 8, 2025, but it is positioned as a metadata source for aggregators and marketplaces, not a magic runtime router; most clients still require user/admin/developer configuration before a model can see and choose a tool ([MCP Registry blog, Sept. 8, 2025](https://blog.modelcontextprotocol.io/posts/2025-09-08-mcp-registry-preview/); [MCP Registry docs, accessed Apr. 21, 2026](https://modelcontextprotocol.io/registry/about)).
- Agent skills are a parallel packaging pattern: Anthropic launched Agent Skills on October 16, 2025 as folders containing instructions, scripts, and resources that Claude loads when relevant; skills are better for procedural know-how and repeatable workflows, while MCP servers are better for live data, authenticated APIs, and computation ([Anthropic, Oct. 16, 2025](https://www.anthropic.com/news/skills?t=n); [Claude Skills docs, accessed Apr. 21, 2026](https://docs.claude.com/en/docs/claude-code/skills)).
- For a calculation-heavy domain like astrology, the core technical pattern is valid: LLMs should call deterministic calculators or authoritative datasets for classes of questions they cannot reliably answer from parameters alone. This pattern is already mainstream in tool calling, database MCP servers, valuation tools, browser automation, sports data, medical calculators, and astrology APIs ([OpenAI function calling docs, accessed Apr. 21, 2026](https://platform.openai.com/docs/guides/function-calling/parallel-function-calling-and-structured-outputs); [Google Cloud MCP Toolbox, Apr. 22, 2025](https://cloud.google.com/blog/products/ai-machine-learning/mcp-toolbox-for-databases-now-supports-model-context-protocol); [Equidam, Aug. 8, 2025](https://www.equidam.com/equidam-mcp-startup-valuation-ai/); [AstrologyAPI, accessed Apr. 21, 2026](https://astrologyapi.com/?s=2025)).
- Monetization is happening mostly through underlying SaaS/API accounts, not through native MCP marketplace revenue share. Paid examples include hosted browser automation, Apify MCP actors, valuation services, astrology APIs, and normal API-metered services wrapped in MCP ([Browserbase, July 22, 2025](https://www.browserbase.com/changelog/browserbase-mcp); [Apify Browserbase MCP listing, accessed Apr. 21, 2026](https://apify.com/mcp-servers/browserbase-mcp-server/api/mcp); [Apify Sports MCP listing, accessed Apr. 21, 2026](https://apify.com/comical_fahrenheit/sports-mcp-server); [AstrologyAPI, accessed Apr. 21, 2026](https://astrologyapi.com/?s=2025)).
- Investment tailwinds are strong but concentrated around agent infrastructure, hosted connectors, sandboxing, browser/computer use, reliability, and distribution layers rather than one-off niche MCP servers. Manufact, formerly `mcp-use`, raised $6.3M seed after YC S2025; YC's 2025-2026 directories show multiple agent-infra companies mentioning MCP, connectors, sandboxes, and universal API access ([VentureBeat, Mar. 11, 2026](https://venturebeat.com/infrastructure/manufact-raises-usd6-3m-as-mcp-becomes-the-usb-c-for-ai-powering-chatgpt-and?lang=en); [YC Infrastructure directory, accessed Apr. 21, 2026](https://www.ycombinator.com/companies/industry/infrastructure)).
- The best near-term founder strategy is likely **API-first plus MCP/skill wrappers**, not "MCP server only." Build a paid deterministic astrology calculation API, publish a high-quality MCP server and Claude/OpenAI skill wrappers, seed registries, and run distribution experiments with measurable evidence: installs, successful tool calls, paid API conversions, retention, and partner interest.
- Publishing an arXiv paper may help credibility and future search/training exposure, but there is no public evidence that it materially causes future frontier models to auto-discover or auto-route to a specific third-party MCP server. Treat the paper route as credibility/content marketing, not a distribution strategy.

## Cluster 1 - Current State of the MCP Ecosystem

### Current-State Summary

MCP is now a young but widely adopted tool/data integration standard for AI applications. Anthropic introduced MCP on November 25, 2024 as an open standard for connecting AI assistants to data sources and tools; this primary launch source is older than 12 months as of April 21, 2026, but it remains the canonical origin record ([Anthropic, Nov. 25, 2024 - older than 12 months](https://www.anthropic.com/news/model-context-protocol?cb=zapier)). Anthropic then donated MCP to the Agentic AI Foundation under the Linux Foundation on December 9, 2025, with Block and OpenAI as co-founders of the foundation and support from Google, Microsoft, AWS, Cloudflare, and Bloomberg ([Anthropic, Dec. 9, 2025](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation?content=Dec2024EOYShips&medium=email&messageTypeId=140367&source=i_email)).

The current public spec revision to anchor against is `2025-11-25`, stable as of the GitHub release surfaced in the MCP repository; its changelog says it follows the `2025-06-18` revision and includes major work in authorization discovery, tool metadata, elicitation, sampling with tool calls, OAuth client metadata, experimental tasks, and governance/process formalization ([GitHub MCP releases, Nov. 25, 2025](https://github.com/modelcontextprotocol/modelcontextprotocol/releases); [MCP changelog, Nov. 25, 2025](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/docs/specification/2025-11-25/changelog.mdx)). The protocol remains JSON-RPC based, with lifecycle management, authorization for HTTP transports, server features such as tools/resources/prompts, client features such as sampling and roots, and utilities such as logging and completion ([MCP spec overview, 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/basic)).

The ecosystem is no longer only Claude Desktop. Claude Code, Cursor, VS Code/GitHub Copilot, Visual Studio, Zed, Gemini CLI, OpenAI's ChatGPT connectors/API integrations, and Codex-style developer tools all support MCP in some form; Anthropic's December 2025 donation post specifically names ChatGPT, Cursor, Gemini, Microsoft Copilot, and Visual Studio Code as adopters ([Anthropic, Dec. 9, 2025](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation?content=Dec2024EOYShips&medium=email&messageTypeId=140367&source=i_email)).

### Named Clients and Configuration Patterns

| Client / surface | MCP support pattern | Current practical significance |
| --- | --- | --- |
| Claude Desktop | Local MCP servers are installed through desktop extensions or configuration; Claude's help center says desktop extensions make local server installation more like browser extensions and that users navigate through Settings > Extensions ([Claude Help Center, updated Apr. 2026](https://support.claude.com/en/articles/10949351-getting-started-with-model-context-protocol-mcp-on-claude-for-desktop)). | Good consumer/prosumer surface for local tools, but user install friction remains unless extensions/directories solve it. |
| Claude Code | `claude mcp add`, `claude mcp list`, `claude mcp get`, and scopes for local/project/user; project-scoped servers are shared through `.mcp.json`; docs also describe dynamic `list_changed`, OAuth for remote servers, resources via `@`, and prompts as `/mcp__server__prompt` commands ([Claude Code docs, accessed Apr. 21, 2026](https://code.claude.com/docs/en/mcp)). | Strongest developer workflow for a solo founder to test demand among agentic coders and power users. |
| Cursor | Supports `stdio`, SSE, and Streamable HTTP transports; configuration lives in `.cursor/mcp.json` for project use or `~/.cursor/mcp.json` globally; Composer Agent automatically uses enabled tools when relevant, with user approval by default ([Cursor docs, accessed Apr. 21, 2026](https://docs.cursor.com/context/model-context-protocol)). | Strong developer distribution, but mostly developer-tool use cases. Domain-specific consumer use depends on users being in Cursor. |
| VS Code / GitHub Copilot | VS Code stores MCP configuration in `.vscode/mcp.json` or user profile; MCP tools can be automatically invoked by agents or explicitly referenced with `#`; VS Code documents a 128-tool-per-request limit and enterprise controls ([VS Code docs, accessed Apr. 21, 2026](https://code.visualstudio.com/docs/copilot/chat/mcp-servers); [VS Code MCP config reference, Apr. 15, 2026](https://code.visualstudio.com/docs/copilot/reference/mcp-configuration)). GitHub's docs say the GitHub MCP server is available to all GitHub users and that Copilot supports local and remote MCP across IDEs, CLI, and cloud agent contexts ([GitHub Docs, accessed Apr. 21, 2026](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp/use-the-github-mcp-server); [GitHub Docs MCP concepts, accessed Apr. 21, 2026](https://docs.github.com/en/copilot/concepts/extensions)). | Large developer distribution and enterprise governance; relevant if the service is useful to builders, analysts, or teams. |
| Visual Studio | Microsoft Learn says Visual Studio 2026 and Visual Studio 2022 17.14 support MCP in Copilot agent mode, including sampling confirmation and admin allowlists ([Microsoft Learn, Mar. 31, 2026](https://learn.microsoft.com/en-us/visualstudio/ide/mcp-servers?view=visualstudio)). | Enterprise .NET reach; less relevant for consumer astrology unless targeting developers. |
| Zed | Zed supports MCP tools and prompts, handles `notifications/tools/list_changed`, and installs servers through extensions or custom settings; the docs warn that model reliability in choosing MCP tools varies and naming the server can help ([Zed docs, accessed Apr. 21, 2026](https://zed.dev/docs/ai/mcp)). | Honest evidence that automatic tool choice is imperfect even inside modern coding agents. |
| Gemini CLI | Gemini CLI discovers tools by iterating configured `mcpServers` in `settings.json`, then connects over stdio/SSE/Streamable HTTP and wraps discovered tools as Gemini function calls ([Gemini CLI docs, accessed Apr. 21, 2026](https://google-gemini.github.io/gemini-cli/docs/tools/mcp-server.html)). | Google ecosystem is participating, but again the server must be configured before discovery occurs. |
| ChatGPT / OpenAI API | ChatGPT custom connectors can be MCP servers; Plus/Pro users can use custom connectors, and Business/Enterprise/Edu workspaces can upload/publish custom connectors subject to permissions; OpenAI's Responses API can use remote MCP servers specified in the `tools` parameter ([OpenAI Help, updated Apr. 2026](https://help.openai.com/en/articles/11487775-connectors-in-chatgpt.webp); [OpenAI MCP docs, accessed Apr. 21, 2026](https://platform.openai.com/docs/mcp/); [OpenAI connectors/API docs, accessed Apr. 21, 2026](https://platform.openai.com/docs/guides/tools-connectors-mcp?lang=javascript)). | Strategically important because it moves MCP beyond Claude; but OpenAI still makes connector publication and activation an explicit product/admin workflow. |

### Server Publishers and Inventory Shape

The official reference-server repository has narrowed its role: it says users looking for lists should browse the MCP Registry, while the repository itself now houses a small number of reference servers maintained by the MCP steering group ([modelcontextprotocol/servers, accessed Apr. 21, 2026](https://github.com/modelcontextprotocol/servers)). Current reference servers include Everything, Fetch, Filesystem, Git, Memory, Sequential Thinking, and Time; previously bundled servers for GitHub, Google Drive, Slack, PostgreSQL, Puppeteer, Redis, Sentry, and SQLite are listed as archived or maintained elsewhere ([modelcontextprotocol/servers, accessed Apr. 21, 2026](https://github.com/modelcontextprotocol/servers)).

The official registry is in preview and stores metadata rather than artifacts. Its docs say it supports publicly accessible servers, uses `server.json`, verifies namespaces through DNS/GitHub-style ownership, and is intended to feed downstream aggregators and marketplaces rather than be directly consumed by host applications ([MCP Registry docs, accessed Apr. 21, 2026](https://modelcontextprotocol.io/registry/about); [MCP Registry repository, Oct. 24, 2025 update](https://github.com/modelcontextprotocol/registry)). The categories represented now include filesystems, source control, cloud databases, developer tools, observability, browser automation, search, productivity apps, payments, calculation services, medical information, sports data, and domain APIs ([modelcontextprotocol/servers, accessed Apr. 21, 2026](https://github.com/modelcontextprotocol/servers); [Smithery docs, accessed Apr. 21, 2026](https://smithery.ai/docs); [Google Cloud, Apr. 22, 2025](https://cloud.google.com/blog/products/ai-machine-learning/mcp-toolbox-for-databases-now-supports-model-context-protocol); [Apify Sports MCP, accessed Apr. 21, 2026](https://apify.com/comical_fahrenheit/sports-mcp-server)).

### Strengths and Limitations

The strengths are portability, enough cross-platform adoption to justify implementation, a clear metadata/registry path, and a low-friction way for existing API providers to become agent-readable. Anthropic's December 2025 claim of more than 10,000 active public servers and adoption across major clients is a strong signal, even though it is a vendor-published metric ([Anthropic, Dec. 9, 2025](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation?content=Dec2024EOYShips&medium=email&messageTypeId=140367&source=i_email)).

The limitations are still material: users/admins usually have to configure a server before the model can see it; authentication and approval UX differ across clients; model tool-choice reliability varies; marketplaces are fragmented; the official registry is preview rather than a hardened app store; tool inventories can exceed model/tool limits; and security risks are rising as MCP servers gain write/action authority ([MCP Registry blog, Sept. 8, 2025](https://blog.modelcontextprotocol.io/posts/2025-09-08-mcp-registry-preview/); [VS Code MCP docs, accessed Apr. 21, 2026](https://code.visualstudio.com/docs/copilot/chat/mcp-servers); [Zed MCP docs, accessed Apr. 21, 2026](https://zed.dev/docs/ai/mcp); [TechRadar, Jan. 21, 2026](https://www.techradar.com/pro/security/anthropics-official-git-mcp-server-had-some-worrying-security-flaws-this-is-what-happened-next); [ITPro, Apr. 2026](https://www.itpro.com/security/ai-agents-using-anthropic-mcp-supply-chain-attacks-claim-researchers)).

### Competing and Adjacent Protocols

MCP does not replace all tool-use mechanisms. OpenAI function calling remains a schema-based API pattern where the developer gives the model function definitions and then executes tool calls in application code ([OpenAI function calling docs, accessed Apr. 21, 2026](https://platform.openai.com/docs/guides/function-calling/parallel-function-calling-and-structured-outputs)). OpenAI's Responses API added support for remote MCP servers in May 2025 while also supporting built-in tools such as image generation, code interpreter, and file search ([OpenAI, May 21, 2025](https://openai.com/index/new-tools-and-features-in-the-responses-api/)). OpenAI Apps in ChatGPT, launched October 6, 2025, are a higher-level product surface where apps can be suggested in conversation or called by name; this is adjacent to MCP because it packages tool access with user-facing interactive UI and platform review/distribution ([OpenAI, Oct. 6, 2025](https://openai.com/index/introducing-apps-in-chatgpt/)).

Google's Gemini CLI uses MCP for tool execution, while Google's broader agent ecosystem also includes function calling and Agent2Agent-style patterns for agent coordination ([Gemini CLI docs, accessed Apr. 21, 2026](https://google-gemini.github.io/gemini-cli/docs/tools/mcp-server.html); [Google Cloud, Apr. 22, 2025](https://cloud.google.com/blog/products/ai-machine-learning/mcp-toolbox-for-databases-now-supports-model-context-protocol)). LangChain, LlamaIndex, Semantic Kernel, and similar frameworks still expose their own tool abstractions, but MCP is increasingly the wire/packaging layer that tool providers can implement once and adapt into multiple hosts.

### Open Issues or Uncertainties

- Whether the official registry becomes a true app-store-like distribution channel or remains upstream metadata for many downstream marketplaces is unresolved; the registry docs currently say host applications should consume downstream registries rather than the official registry directly ([MCP Registry docs, accessed Apr. 21, 2026](https://modelcontextprotocol.io/registry/about)).
- Security posture is unsettled because MCP servers can bridge prompt inputs to local files, terminals, browsers, databases, or cloud services; security research in early 2026 reported risks around official Git/Filesystem server chains and broader MCP supply-chain patterns ([TechRadar, Jan. 21, 2026](https://www.techradar.com/pro/security/anthropics-official-git-mcp-server-had-some-worrying-security-flaws-this-is-what-happened-next); [ITPro, Apr. 2026](https://www.itpro.com/security/ai-agents-using-anthropic-mcp-supply-chain-attacks-claim-researchers)).
- Client capability support is uneven. For example, Zed says it supports tools and prompts but welcomes contributions for discovery, sampling, and elicitation; Cursor and Claude Code support more features, but with different configuration files and approval behavior ([Zed docs, accessed Apr. 21, 2026](https://zed.dev/docs/ai/mcp); [Cursor docs, accessed Apr. 21, 2026](https://docs.cursor.com/context/model-context-protocol); [Claude Code docs, accessed Apr. 21, 2026](https://code.claude.com/docs/en/mcp)).
- There is no universal guarantee that an LLM will choose the right tool merely because it is installed. Zed's docs explicitly say reliability can vary by model and that naming the MCP server can help ([Zed docs, accessed Apr. 21, 2026](https://zed.dev/docs/ai/mcp)).

### Implications for the Founder

- MCP is now credible enough to justify a production-quality wrapper for an existing calculation service.
- MCP alone is not distribution. A server must still be installed, approved, listed in a connector catalog, bundled in a skill/plugin, or called by a developer through an API.
- A deterministic calculation engine is a good fit for MCP because it returns grounded data that the LLM should not hallucinate.
- Avoid depending on one client. Package for Claude, ChatGPT/OpenAI API, Cursor/VS Code, and generic HTTP MCP clients.
- Treat security, rate limiting, audit logs, and user-visible provenance as product features, not infrastructure chores.

## Cluster 2 - Agent Skills as a Parallel / Complementary Pattern

### Current-State Summary

Agent skills are a packaging pattern for procedural knowledge, not a protocol for remote tool execution. Anthropic introduced Agent Skills on October 16, 2025 as folders containing instructions, scripts, and resources that Claude loads when relevant; Anthropic says skills are composable and designed to avoid loading all details into the context window upfront ([Anthropic, Oct. 16, 2025](https://www.anthropic.com/news/skills?t=n)). Claude's help center describes skills as dynamically loaded folders that improve performance on specialized tasks and use progressive disclosure to prevent context-window overload ([Claude Help Center, accessed Apr. 21, 2026](https://support.claude.com/en/articles/12512176-what-are-skills)).

In Claude Code, each skill is a directory with a required `SKILL.md` containing YAML frontmatter and Markdown instructions; `name` and `description` are the discovery-critical fields, and optional files can include scripts, templates, references, and examples ([Claude Code Skills docs, accessed Apr. 21, 2026](https://docs.claude.com/en/docs/claude-code/skills)). Claude Code supports personal skills in `~/.claude/skills/`, project skills in `.claude/skills/`, and plugin skills bundled by plugins ([Claude Code Skills docs, accessed Apr. 21, 2026](https://docs.claude.com/en/docs/claude-code/skills)). Anthropic's API docs also expose Skills endpoints behind the `skills-2025-10-02` beta header, including create/list/get operations ([Claude API Skills docs, accessed Apr. 21, 2026](https://docs.claude.com/en/api/skills); [Claude API List Skills, accessed Apr. 21, 2026](https://docs.claude.com/en/api/skills/list-skills)).

### How Skills Differ from MCP Servers

MCP servers expose callable tools, resources, and prompts over a protocol; skills tell the agent how to perform a task, when to use certain resources, how to format outputs, and which scripts/templates to consult. A skill can include code, but it is loaded as part of an agent workflow, whereas an MCP server is a live service boundary that can perform authenticated calls, retrieve data, or run deterministic computation ([MCP spec overview, 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/basic); [Anthropic Skills engineering post, Oct. 16, 2025](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills?_bhlid=fb54f3ffd4d8cdcde5a43e105d8980dbb53abb65)).

The emerging unification pattern is **skill or plugin as the wrapper and MCP as the execution substrate**. Claude Code's MCP docs say plugins can include MCP server configuration in `.mcp.json` or inline `plugin.json`, and that plugin MCP servers start automatically when the plugin is enabled ([Claude Code MCP docs, accessed Apr. 21, 2026](https://docs.claude.com/en/docs/claude-code/mcp)). Claude Code's skills docs say plugins can bundle skills that become available when installed ([Claude Code Skills docs, accessed Apr. 21, 2026](https://docs.claude.com/en/docs/claude-code/skills)). VS Code custom agents can declare available tools and `mcp-servers` in agent frontmatter for GitHub Copilot-targeted agents, which is another example of instructions plus tool wiring in one artifact ([VS Code custom agents docs, accessed Apr. 21, 2026](https://code.visualstudio.com/docs/copilot/chat/chat-modes)).

### OpenAI, Google, and Third-Party Equivalents

OpenAI's closest user-facing analogs are GPTs, custom actions, connectors, and Apps in ChatGPT. GPTs are custom versions of ChatGPT with instructions, knowledge, capabilities, apps, and actions; Enterprise docs say the GPT Store is where users search and access GPTs, subject to workspace policy ([OpenAI Help, updated Apr. 2026](https://help.openai.com/en/articles/8555535-gpts-chatgpt-enterprise-version)). GPT actions connect a GPT to external APIs through authentication plus an OpenAPI schema, and public GPTs with actions require a privacy policy URL ([OpenAI Help, updated Apr. 2026](https://help.openai.com/en/articles/9442513-configuring-actions-in-gpts)). ChatGPT custom connectors use MCP to expose third-party or internal systems to ChatGPT chat and deep research; custom connectors must be explicitly enabled and are not verified by OpenAI in developer mode ([OpenAI Help, updated Apr. 2026](https://help.openai.com/en/articles/11487775-connectors-in-chatgpt.webp)).

OpenAI Apps in ChatGPT are more like a conversational app platform than a simple skill. OpenAI launched Apps SDK preview on October 6, 2025, with apps from Booking.com, Canva, Coursera, Expedia, Figma, Spotify, and Zillow; OpenAI says users can discover apps when ChatGPT suggests one at the right time or by calling apps by name ([OpenAI, Oct. 6, 2025](https://openai.com/index/introducing-apps-in-chatgpt/)). This matters because platform-native app UX may become a better distribution surface than raw MCP for consumer-facing domains.

Google has MCP support in Gemini CLI and Google Cloud database tooling, but it does not currently expose a Claude-style portable `SKILL.md` standard as the primary agent skill artifact in the sources reviewed ([Gemini CLI docs, accessed Apr. 21, 2026](https://google-gemini.github.io/gemini-cli/docs/tools/mcp-server.html); [Google Cloud, Apr. 22, 2025](https://cloud.google.com/blog/products/ai-machine-learning/mcp-toolbox-for-databases-now-supports-model-context-protocol)). Google Chrome's April 2026 consumer "Skills" feature, reported by The Verge, is a prompt reuse feature rather than an MCP-equivalent execution protocol; because this is a news report and not a developer standard, it should not be treated as directly comparable to Anthropic Agent Skills ([The Verge, Apr. 2026](https://www.theverge.com/tech/911658/google-chrome-gemini-ai-skills-availability-launch)).

### When a Capability Belongs as Skill, MCP Server, or Both

Use a skill when the value is procedural: how to interpret outputs, how to ask clarifying questions, how to structure a reading, how to choose house systems, how to cite caveats, how to write in a brand voice, or how to combine multiple existing tools. Use an MCP server when the value is live or executable: ephemeris calculations, birth-chart generation, transits, progressions, void-of-course periods, compatibility calculations, account-specific saved charts, billing, audit logs, or dataset lookups. Use both when the system needs deterministic data plus domain-specific interpretation discipline.

For AstroPrompt-like infrastructure, a plausible packaging stack is: deterministic calculation API -> remote MCP server -> Claude skill / GPT instructions / ChatGPT app / VS Code custom agent profile that teaches the agent when to call the calculator and how to interpret the returned schema. This follows the emerging pattern in Claude Code plugins and VS Code custom agents where procedural instructions and MCP server wiring travel together ([Claude Code MCP docs, accessed Apr. 21, 2026](https://docs.claude.com/en/docs/claude-code/mcp); [VS Code custom agents docs, accessed Apr. 21, 2026](https://code.visualstudio.com/docs/copilot/chat/chat-modes)).

### Open Issues or Uncertainties

- Anthropic says Skills are now an open standard in its engineering post update, but public cross-vendor portability is still early compared with MCP client adoption ([Anthropic engineering, Oct. 16 / Dec. 18, 2025 update](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills?_bhlid=fb54f3ffd4d8cdcde5a43e105d8980dbb53abb65)).
- Skills have security risks because they can include instructions and scripts; Claude's docs strongly recommend using skills only from trusted sources ([Claude Agent Skills overview, accessed Apr. 21, 2026](https://docs.claude.com/en/docs/agents-and-tools/agent-skills)).
- OpenAI GPTs/actions, ChatGPT Apps, and custom MCP connectors are overlapping but distinct distribution systems; there is no single "publish once as a skill everywhere" marketplace today ([OpenAI Help GPTs, updated Apr. 2026](https://help.openai.com/en/articles/8555535-gpts-chatgpt-enterprise-version); [OpenAI Apps, Oct. 6, 2025](https://openai.com/index/introducing-apps-in-chatgpt/); [OpenAI MCP docs, accessed Apr. 21, 2026](https://platform.openai.com/docs/mcp/)).

### Implications for the Founder

- A domain calculation product should not choose between MCP and skills; the better architecture is both.
- The MCP server should own calculation, authentication, metering, and data provenance.
- The skill should own domain routing, interpretation discipline, prompt templates, and user-facing caveats.
- A high-quality `description` in both the skill and MCP tool schemas is strategically important because model-side selection depends heavily on metadata.
- Skills can become a low-friction way to distribute "how to use AstroPrompt correctly" even when the API remains proprietary.

## Cluster 3 - Tool Use and Specialized Calculation in LLM Apps Generally

### Current-State Summary

The broader tool-use pattern is mature: developers give a model a set of tools, the model selects a tool call, application code or a remote service executes it, and the model synthesizes the result. OpenAI's function calling docs describe this five-step loop and explicitly define tools as functionality given to the model, tool calls as model requests, and tool outputs as the application-provided response ([OpenAI function calling docs, accessed Apr. 21, 2026](https://platform.openai.com/docs/guides/function-calling/parallel-function-calling-and-structured-outputs)). Structured Outputs, launched in June 2024, enforce JSON Schema conformance for function arguments when `strict: true`; this source is older than 12 months but remains the official OpenAI history for that feature ([OpenAI Help, updated Apr. 2026; feature date June 2024 - older than 12 months](https://help.openai.com/en/articles/8555517-function-calling-in-the-openai-api%23.eps)).

MCP is one layer in this broader pattern. OpenAI's May 21, 2025 Responses API update added support for remote MCP servers alongside built-in tools like image generation, Code Interpreter, and file search ([OpenAI, May 21, 2025](https://openai.com/index/new-tools-and-features-in-the-responses-api/)). Google Cloud's MCP Toolbox for Databases exposes enterprise databases through an open-source MCP server, which is a concrete example of wrapping authoritative data behind an agent-readable tool boundary ([Google Cloud, Apr. 22, 2025](https://cloud.google.com/blog/products/ai-machine-learning/mcp-toolbox-for-databases-now-supports-model-context-protocol)).

### Cases Where LLMs Need External Calculation or Data

| Domain | Why the LLM needs a tool | Concrete examples and dates | What worked / did not work |
| --- | --- | --- | --- |
| Math and computation | LLMs can reason but still make arithmetic, symbolic, and numeric errors; calculators or code execution provide deterministic outputs. | OpenAI's Responses API includes Code Interpreter as a built-in tool as of May 21, 2025; MCPCalc advertises connected math workspaces and MCP integration for shared sessions as of April 2026 ([OpenAI, May 21, 2025](https://openai.com/index/new-tools-and-features-in-the-responses-api/); [MCPCalc, accessed Apr. 21, 2026](https://mcpcalc.com/)). Wolfram-style LLM integrations are important prior art, but most primary ChatGPT-plugin evidence is older than 12 months. | Tool execution works when outputs are structured, verifiable, and bounded. Plugin/app-store distribution has been less stable than API/tool primitives. |
| Databases and enterprise data | Models cannot know private/current database state from pretraining. | Google Cloud's MCP Toolbox supports BigQuery, AlloyDB, Cloud SQL, Spanner, self-managed PostgreSQL/MySQL/SQLite, Neo4j, Dgraph, and other vendors as of June 4, 2025 ([Google Cloud, June 4, 2025](https://cloud.google.com/blog/products/ai-machine-learning/new-mcp-integrations-to-google-cloud-databases)). | Strong fit for MCP because database access is structured, permissioned, and high value. |
| Developer workflows | Models need live repo, issue, CI, browser, and cloud context. | GitHub's MCP server is maintained by GitHub and works in Copilot Chat; Browserbase introduced a hosted browser MCP on July 22, 2025; Cursor, VS Code, Claude Code, and Zed all document MCP usage in coding agents ([GitHub Docs, accessed Apr. 21, 2026](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp/use-the-github-mcp-server); [Browserbase, July 22, 2025](https://www.browserbase.com/changelog/browserbase-mcp); [Cursor docs, accessed Apr. 21, 2026](https://docs.cursor.com/context/model-context-protocol)). | This is currently MCP's strongest market because users already accept tool installation, permissions, and logs in developer workflows. |
| Finance / valuation | Models need current market/account data and deterministic formulas. | Equidam launched an MCP server on August 8, 2025 for startup valuation and says its platform had valued more than 140,000 companies across 90+ countries ([Equidam, Aug. 8, 2025](https://www.equidam.com/equidam-mcp-startup-valuation-ai/)). | Existing B2B APIs can add MCP as another interface; traction is usually from the core SaaS, not the MCP wrapper itself. |
| Sports | Models need current schedules, scores, rosters, and stats. | Apify's Sports MCP Server advertises access to 10 data sources through one endpoint and uses Apify token-based access as of April 2026 ([Apify Sports MCP, accessed Apr. 21, 2026](https://apify.com/comical_fahrenheit/sports-mcp-server)). | Works if data licensing and freshness are solved; public proof of large adoption for independent sports MCP servers is thin. |
| Medical and scientific information | Models need authoritative, current, source-cited information and calculators; risk is high. | A public Healthcare MCP server advertises FDA drug info, PubMed, medRxiv, NCBI Bookshelf, clinical trials, ICD-10, DICOM metadata, and a medical calculator as of August 2025; MedCalcMCP advertises 56 professional medical calculators and unit conversion in a directory listing as of April 2026 ([healthcare-mcp-public GitHub, 2025](https://github.com/Cicatriiz/healthcare-mcp-public); [MedCalcMCP directory, accessed Apr. 21, 2026](https://mcp.aibase.com/server/1574493273580642488)). | High-value but high-liability. Trust, credentialing, clinical disclaimers, and source provenance matter more than generic MCP compatibility. |
| Astrology | LLMs need ephemeris-grade calculations and chart state; interpretations are sensitive to exact birth time/place and house/aspect rules. | AstrologyAPI advertises birth charts, Kundli, transits, horoscopes, AI readings, 300+ endpoints, 3,000+ active businesses, 100M+ daily API calls, and an MCP server connecting Claude/GPT/LLMs to real ephemeris data as of April 2026 ([AstrologyAPI, accessed Apr. 21, 2026](https://astrologyapi.com/?s=2025)). | This is direct evidence that the AstroPrompt hypothesis is not unique; a competitor/API provider is already positioning MCP as an ephemeris-data bridge. |

### Case Study Pattern: What Makes Integrations Work

Successful integrations share four traits: they return data the model cannot already know, they provide structured outputs with clear semantics, they live where users already work, and they have a trust/auth/payment model outside the LLM. Google Cloud's database toolbox works because it brings governed enterprise data into agent workflows; GitHub's MCP server works because it sits inside Copilot/IDE workflows; Browserbase works because browser sessions are a scarce external capability; Equidam works because valuation logic is a specialized tool attached to an existing paid platform ([Google Cloud, Apr. 22, 2025](https://cloud.google.com/blog/products/ai-machine-learning/mcp-toolbox-for-databases-now-supports-model-context-protocol); [GitHub Docs, accessed Apr. 21, 2026](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp/use-the-github-mcp-server); [Browserbase, July 22, 2025](https://www.browserbase.com/changelog/browserbase-mcp); [Equidam, Aug. 8, 2025](https://www.equidam.com/equidam-mcp-startup-valuation-ai/)).

The weaker cases are generic calculators, thin wrappers around public APIs, and tools that require the user to install/configure a server before they feel any benefit. A scientific calculator MCP listing with zero stars and sparse visible activity is a warning that "calculation capability" alone is not enough; demand depends on audience, distribution, trust, and workflow fit ([Scientific Calculator MCP listing, accessed Apr. 21, 2026](https://mcpmarket.com/server/scientific-calculator)).

### Open Issues or Uncertainties

- Public adoption numbers are rarely attributable to the MCP wrapper alone; most paid providers publish overall platform metrics rather than MCP-specific usage.
- Domain calculators in regulated or quasi-regulated categories need stronger provenance and error handling than general developer tools.
- LLMs still need routing guidance: a correct calculation API does not help if the model fails to call it, passes incomplete birth data, or ignores output caveats.

### Implications for the Founder

- Astrology calculation is a legitimate external-tool category because exact ephemeris/chart computation is outside normal LLM reliability.
- The product should return structured data plus interpretation-ready summaries, not only prose.
- Build tests around "model fails to call the calculator" and "model calls with insufficient inputs"; routing failure is as important as calculation failure.
- Differentiation should be domain depth, schemas, interpretation discipline, and workflow packaging, not "we have an MCP server."
- Existing AstrologyAPI's MCP positioning means speed matters, but a focused, high-quality niche product can still win on accuracy, ergonomics, and community trust.

## Cluster 4 - Discovery Mechanisms, the Hard Problem

### Current-State Summary

In April 2026, discovery is mostly **manual or semi-automatic after configuration**, not automatic global inference-time discovery. The pattern is: a user/admin/developer installs or specifies a server; the client lists tools from that server; the model chooses among available tools; the user or policy layer approves calls. Cursor says Agent uses MCP tools listed under Available Tools when relevant, and users can enable/disable tools from chat ([Cursor docs, accessed Apr. 21, 2026](https://docs.cursor.com/context/model-context-protocol)). Gemini CLI says it iterates configured `mcpServers`, establishes connections, fetches tool definitions, sanitizes schemas, and registers tools in a local registry ([Gemini CLI docs, accessed Apr. 21, 2026](https://google-gemini.github.io/gemini-cli/docs/tools/mcp-server.html)). OpenAI says the Responses API attempts to list tools only when a remote MCP server is specified in the `tools` parameter ([OpenAI connectors/API docs, accessed Apr. 21, 2026](https://platform.openai.com/docs/guides/tools-connectors-mcp?lang=javascript)).

ChatGPT connectors are more user-friendly but still configured. OpenAI's help center says users enable connectors through Settings > Apps & Connectors, and custom MCP connectors require developer mode for Plus/Pro or admin/owner publishing in Business/Enterprise/Edu workspaces ([OpenAI Help, updated Apr. 2026](https://help.openai.com/en/articles/11487775-connectors-in-chatgpt.webp)). OpenAI also says synced connectors can be referenced automatically when ChatGPT thinks they help, but that is automatic use of already connected/indexed sources, not autonomous discovery of arbitrary third-party MCP servers ([OpenAI Help, updated Apr. 2026](https://help.openai.com/en/articles/11487775-connectors-in-chatgpt.webp)).

### Registries, Hubs, and Marketplaces

The official MCP Registry launched in preview on September 8, 2025 as an open catalog/API for publicly available MCP servers ([MCP Registry blog, Sept. 8, 2025](https://blog.modelcontextprotocol.io/posts/2025-09-08-mcp-registry-preview/)). Its docs say it provides one place for metadata publication, namespace management through DNS verification, a REST API for clients/aggregators, and standardized installation/configuration data ([MCP Registry docs, accessed Apr. 21, 2026](https://modelcontextprotocol.io/registry/about)). The registry intentionally hosts metadata, not packages; package artifacts live on npm, PyPI, Docker Hub, NuGet, or remote server URLs, and the registry maps server names to those artifacts ([MCP Registry package types, accessed Apr. 21, 2026](https://modelcontextprotocol.io/registry/package-types)).

Smithery is a major downstream marketplace. Its docs describe it as the largest open marketplace of MCP servers, list examples such as Exa, Context7, and Browserbase, and offer Smithery Connect for managed OAuth/credentials plus server publishing and observability ([Smithery docs, accessed Apr. 21, 2026](https://smithery.ai/docs)). Community directories such as MCP.so, PulseMCP, mcpservers.org, MCP Server Finder, and Apify's MCP listings provide discovery surfaces, but their metadata quality, trust model, and business models vary ([MCP Registry docs on aggregators, accessed Apr. 21, 2026](https://modelcontextprotocol.io/registry/registry-aggregators); [Smithery docs, accessed Apr. 21, 2026](https://smithery.ai/docs); [Apify Browserbase MCP listing, accessed Apr. 21, 2026](https://apify.com/mcp-servers/browserbase-mcp-server/api/mcp)).

The closest analog to DNS/npm/pip is a layered system: official registry for server metadata, package registries for artifacts, marketplaces for curation/ratings/security, and client-specific installers for actual activation. The official registry's own docs describe this layered relationship and say downstream aggregators can add ratings, download counts, and security scans in `_meta` fields ([MCP Registry docs, accessed Apr. 21, 2026](https://modelcontextprotocol.io/registry/about); [MCP Registry Aggregators, accessed Apr. 21, 2026](https://modelcontextprotocol.io/registry/registry-aggregators)).

### Role of Model Training Data

Base-model awareness of a server is not the same as runtime access. OpenAI's own Docs MCP page advises adding an `AGENTS.md` instruction telling the agent to use the OpenAI developer docs MCP server because otherwise the user may need to explicitly tell the agent to consult it; it also says short, descriptive server names help the agent select the server ([OpenAI Docs MCP, accessed Apr. 21, 2026](https://platform.openai.com/docs/docs-mcp)). This is practical evidence that even a first-party, public, well-known MCP server benefits from runtime instructions and configured access.

Publishing docs or a paper may help humans find the server, search engines index it, and future models mention it after training, but there is no public evidence that an arXiv paper causes frontier models to autonomously discover or route to a third-party MCP server at inference time. Current clients expose configured tools, not the open internet of possible tools, to the model context ([Cursor docs, accessed Apr. 21, 2026](https://docs.cursor.com/context/model-context-protocol); [Gemini CLI docs, accessed Apr. 21, 2026](https://google-gemini.github.io/gemini-cli/docs/tools/mcp-server.html); [OpenAI connectors/API docs, accessed Apr. 21, 2026](https://platform.openai.com/docs/guides/tools-connectors-mcp?lang=javascript)).

### Academic and Open-Source Work on Automatic Tool Discovery

The most directly relevant recent paper found in this pass is **MCP-Zero: Active Tool Discovery for Autonomous LLM Agents**, published June 1, 2025, which frames tool use as active on-demand discovery rather than static schema injection and evaluates against an MCP-tools dataset of 308 servers and 2,797 tools ([MCP-Zero summary, June 1, 2025](https://www.sciencestack.ai/paper/2506.01056v4)). The paper's reported mechanisms, Active Tool Request, Hierarchical Semantic Routing, and Iterative Capability Extension, match the founder's intuition about a classifier/router layer, but this is research evidence rather than proof of mainstream product behavior ([MCP-Zero summary, June 1, 2025](https://www.sciencestack.ai/paper/2506.01056v4)).

Older tool-use research such as Toolformer, Gorilla, ToolLLM, and APIBench remains conceptually relevant but is older than 12 months as of April 2026, so it should be treated as prior art rather than current market evidence.

### Open Issues or Uncertainties

- The official registry has preview/data-reset caveats and is not yet equivalent to npm for runtime install/use ([MCP Registry docs, accessed Apr. 21, 2026](https://modelcontextprotocol.io/registry/about)).
- Downstream marketplaces may become the real discovery layer, but their economics and trust models are not settled.
- Automatic tool selection among already configured tools is improving, but automatic global discovery remains mostly a research/infrastructure problem.
- Future model training may increase name recognition for popular servers, but name recognition does not solve authentication, installation, policy approval, or routing.

### Implications for the Founder

- Do not assume "publish MCP server, wait for models to know it" will work.
- Publish to registries and directories, but measure installs, successful calls, and paid conversions.
- Create high-quality metadata: short names, precise descriptions, JSON schemas, examples, and negative-use guidance.
- Bundle the server with a skill or GPT/App wrapper that teaches the agent when to call it.
- Consider a router/classifier layer, but start with explicit instructions and tool schemas before building a generalized marketplace.

## Cluster 5 - Monetization Patterns

### Current-State Summary

MCP monetization in 2026 is mostly **API/SaaS monetization with an MCP interface**, not a mature MCP app-store economy. OpenAI's ChatGPT connector docs describe custom MCP connectors across plans, but they do not describe a general revenue-share program for third-party MCP server providers ([OpenAI Help, updated Apr. 2026](https://help.openai.com/en/articles/11487775-connectors-in-chatgpt.webp)). Anthropic's Skills and MCP docs describe creation, upload, and use, but the reviewed primary docs do not expose a broad public marketplace payout mechanism for third-party skills or servers ([Claude API Skills docs, accessed Apr. 21, 2026](https://docs.claude.com/en/api/skills); [Claude Code MCP docs, accessed Apr. 21, 2026](https://docs.claude.com/en/docs/claude-code/mcp)).

The strongest paid pattern is: the server authenticates to an existing paid service, uses the service's billing model, and MCP becomes a distribution/integration layer. Browserbase's July 22, 2025 MCP launch advertises a hosted remote server for parallel browser sessions, and Apify's Browserbase MCP listing is explicitly "pay per event" ([Browserbase, July 22, 2025](https://www.browserbase.com/changelog/browserbase-mcp); [Apify Browserbase MCP listing, accessed Apr. 21, 2026](https://apify.com/mcp-servers/browserbase-mcp-server/api/mcp)). Apify's Sports MCP uses one Apify token and runs tools as Apify Actors, again monetizing through the platform rather than an MCP-native revenue share ([Apify Sports MCP, accessed Apr. 21, 2026](https://apify.com/comical_fahrenheit/sports-mcp-server)).

### Concrete Paid or Commercial Examples

| Provider / server | Date | Monetization evidence | Adoption / traction signal | Relevance |
| --- | --- | --- | --- | --- |
| Browserbase MCP | July 22, 2025 | Browserbase offers hosted browser sessions; Apify listing prices the Browserbase MCP as pay-per-event ([Browserbase, July 22, 2025](https://www.browserbase.com/changelog/browserbase-mcp); [Apify listing, accessed Apr. 21, 2026](https://apify.com/mcp-servers/browserbase-mcp-server/api/mcp)). | Browserbase itself is a known AI browser-infra provider; MCP-specific public usage is not broken out. | Strong example of "scarce external capability + usage billing." |
| Apify Sports MCP | April 2026 listing | Uses an Apify token and Apify Actors; Apify's business model supports actor usage billing ([Apify Sports MCP, accessed Apr. 21, 2026](https://apify.com/comical_fahrenheit/sports-mcp-server)). | Directory-level listing, not broad revenue proof. | Useful for domain data wrappers with paid platform rails. |
| Equidam Valuation MCP | Aug. 8, 2025 | MCP exposes Equidam's professional valuation tools; the underlying platform is the commercial product ([Equidam, Aug. 8, 2025](https://www.equidam.com/equidam-mcp-startup-valuation-ai/)). | Equidam claims 140,000+ companies valued across 90+ countries ([Equidam, Aug. 8, 2025](https://www.equidam.com/equidam-mcp-startup-valuation-ai/)). | Strong comparable for specialized calculation as a service. |
| AstrologyAPI MCP | April 2026 | AstrologyAPI sells astrology API access and advertises MCP server access to real ephemeris data ([AstrologyAPI, accessed Apr. 21, 2026](https://astrologyapi.com/?s=2025)). | Claims 300+ endpoints, 3,000+ active businesses, 100M+ daily API calls; these are vendor claims and not independently audited in this pass ([AstrologyAPI, accessed Apr. 21, 2026](https://astrologyapi.com/?s=2025)). | Direct competitor/validation for astrology calculation infrastructure. |
| Stripe / PayPal-style remote MCP | 2025-2026 docs examples | Claude Code docs use Stripe and PayPal MCP URLs in examples, implying remote MCP endpoints with existing account auth and payment-platform economics ([Claude Code MCP docs, accessed Apr. 21, 2026](https://docs.claude.com/en/docs/claude-code/mcp)). | No MCP-specific revenue metrics. | Shows how major API platforms can make MCP an account-authenticated interface. |

### Platform Revenue Share Status

OpenAI introduced the GPT Store on January 10, 2024 and said builders could earn based on GPT usage; this source is older than 12 months and refers to GPTs, not MCP servers ([OpenAI, Jan. 10, 2024 - older than 12 months](https://openai.com/index/introducing-the-gpt-store/)). OpenAI's October 2025 Apps in ChatGPT launch created a more conversational app surface and press coverage reported that OpenAI was exploring commerce/monetization options, but the reviewed primary OpenAI announcement emphasizes app UX and developer preview rather than a mature public payout system ([OpenAI, Oct. 6, 2025](https://openai.com/index/introducing-apps-in-chatgpt/); [TechCrunch, Oct. 6, 2025](https://techcrunch.com/2025/10/06/openai-launches-apps-inside-of-chatgpt/)).

Anthropic has connector directories, Skills, Claude Code plugins, and MCP support, but the reviewed Anthropic docs do not show a broad third-party revenue-share marketplace for skills or MCP servers as of April 2026 ([Anthropic Skills, Oct. 16, 2025](https://www.anthropic.com/news/skills?t=n); [Claude Code Skills docs, accessed Apr. 21, 2026](https://docs.claude.com/en/docs/claude-code/skills); [Claude Desktop MCP help, updated Apr. 2026](https://support.claude.com/en/articles/10949351-getting-started-with-model-context-protocol-mcp-on-claude-for-desktop)).

### Payment Rails and Authentication Patterns

The dominant patterns are OAuth for remote servers, API keys or environment variables for local stdio servers, platform tokens such as Apify tokens, and normal SaaS account billing behind the server. Cursor supports OAuth for SSE/Streamable HTTP servers and environment variables for API keys ([Cursor docs, accessed Apr. 21, 2026](https://docs.cursor.com/context/model-context-protocol)). Claude Code supports OAuth login through `/mcp` for remote servers and environment variables in configuration ([Claude Code docs, accessed Apr. 21, 2026](https://code.claude.com/docs/en/mcp)). OpenAI custom connectors require users to authenticate with each connector and allow Enterprise/Business admins to publish custom connectors for workspace use ([OpenAI Help, updated Apr. 2026](https://help.openai.com/en/articles/11487775-connectors-in-chatgpt.webp)). VS Code and GitHub Copilot have enterprise policy controls and allowlists for MCP usage ([VS Code docs, accessed Apr. 21, 2026](https://code.visualstudio.com/docs/copilot/chat/mcp-servers); [GitHub Docs, accessed Apr. 21, 2026](https://docs.github.com/en/copilot/concepts/extensions)).

### Adjacent Precedents and Cautions

The GPT Store and ChatGPT plugin history are cautionary because platform distribution can change quickly; OpenAI's 2025 Apps launch followed GPTs and earlier plugin-style attempts, with a new app directory and in-chat invocation model ([OpenAI GPT Store, Jan. 10, 2024 - older than 12 months](https://openai.com/index/introducing-the-gpt-store/); [OpenAI Apps, Oct. 6, 2025](https://openai.com/index/introducing-apps-in-chatgpt/); [TechCrunch, Oct. 6, 2025](https://techcrunch.com/2025/10/06/openai-launches-apps-inside-of-chatgpt/)). App Stores, Chrome extensions, Zapier integrations, Discord bot directories, and Shopify app marketplaces suggest that the durable business is often a combination of install/discovery surface, review/trust layer, billing, analytics, and vendor support rather than the extension format alone.

### Open Issues or Uncertainties

- I did not find public, audited revenue numbers for MCP-server-first businesses; most evidence is platform funding, directory activity, or underlying SaaS traction.
- Native skill/server revenue share from Anthropic or OpenAI appears either absent, limited, or not publicly mature as of April 2026 in the reviewed sources.
- Payment middleware for MCP exists as an emerging category, but no single payment standard appears dominant in the reviewed evidence.

### Implications for the Founder

- Charge for the API/service, not for "the MCP server" as a standalone artifact.
- Use MCP as a paid acquisition and integration channel.
- Offer a free/dev tier to reduce install friction, then meter calculation volume, saved profiles, batch jobs, commercial use, or premium interpretations.
- Prefer OAuth or account-token auth for remote MCP; use local env vars only for developer/prototype workflows.
- Build usage analytics from day one: installs, auth completions, calculation calls, failed calls, conversion, and retention by client surface.

## Cluster 6 - Comparable Domain-Specific Plays

### Current-State Summary

The comparable market is not "MCP servers for astrology"; it is "specialized capability providers exposing authoritative data/calculation to agents." Evidence exists across valuation, browser automation, databases, sports, medical information, calculators, and astrology APIs, but traction varies widely and is often not MCP-specific.

### Domain-Specific Examples

| Domain | Example | Date | What they built | Who pays / distribution | Current traction signal |
| --- | --- | --- | --- | --- | --- |
| Startup valuation | Equidam MCP Server | Aug. 8, 2025 | Professional startup valuation tools exposed through MCP ([Equidam, Aug. 8, 2025](https://www.equidam.com/equidam-mcp-startup-valuation-ai/)). | Equidam's existing valuation platform users; MCP as AI integration channel. | Vendor claims 140,000+ companies valued in 90+ countries ([Equidam, Aug. 8, 2025](https://www.equidam.com/equidam-mcp-startup-valuation-ai/)). |
| Enterprise databases | Google Cloud MCP Toolbox for Databases | Apr. 22 and June 4, 2025 | Open-source MCP server connecting agents to enterprise databases ([Google Cloud, Apr. 22, 2025](https://cloud.google.com/blog/products/ai-machine-learning/mcp-toolbox-for-databases-now-supports-model-context-protocol); [Google Cloud, June 4, 2025](https://cloud.google.com/blog/products/ai-machine-learning/new-mcp-integrations-to-google-cloud-databases)). | Cloud/database usage and enterprise workflows. | Major cloud vendor backing; broad supported database list. |
| Browser automation | Browserbase MCP | July 22, 2025 | Hosted browser automation via Stagehand-powered MCP ([Browserbase, July 22, 2025](https://www.browserbase.com/changelog/browserbase-mcp)). | Usage-based browser infrastructure; distribution via GitHub, Smithery/marketplaces, Apify-style listings. | Public changelog; Apify pay-per-event listing ([Apify Browserbase MCP, accessed Apr. 21, 2026](https://apify.com/mcp-servers/browserbase-mcp-server/api/mcp)). |
| Sports data | Apify Sports MCP Server | Apr. 2026 listing | Ten specialized sports data tools behind a single MCP endpoint ([Apify Sports MCP, accessed Apr. 21, 2026](https://apify.com/comical_fahrenheit/sports-mcp-server)). | Apify token/actor usage. | Directory listing; no audited adoption found. |
| Healthcare / medical info | healthcare-mcp-public | 2025 | FDA drug info, PubMed, medRxiv, NCBI Bookshelf, clinical trials, ICD-10, DICOM metadata, and calculator tools ([GitHub, 2025](https://github.com/Cicatriiz/healthcare-mcp-public)). | Open-source / developer use; unclear production buyer. | Public repo; no validated adoption in this pass. |
| Medical calculators | MedCalcMCP | Apr. 2026 listing | 56 medical calculators, unit conversion, explanations ([MCP Aibase, accessed Apr. 21, 2026](https://mcp.aibase.com/server/1574493273580642488)). | Directory service / unknown commercial model. | Directory shows 4.7K-style activity metric, but the metric is not independently interpreted. |
| General calculators | Scientific Calculator MCP | Apr. 2026 listing | Atomic scientific calculator tools ([MCPMarket, accessed Apr. 21, 2026](https://mcpmarket.com/server/scientific-calculator)). | Sponsored listing / unclear buyer. | Listing shows no strong adoption signal; cautionary example. |
| Astrology API | AstrologyAPI MCP | Apr. 2026 | Astrology APIs plus MCP bridge for Claude/GPT/LLMs to real ephemeris data ([AstrologyAPI, accessed Apr. 21, 2026](https://astrologyapi.com/?s=2025)). | API customers, AI builders, astrology businesses. | Vendor claims 300+ endpoints, 3,000+ active businesses, and 100M+ daily API calls ([AstrologyAPI, accessed Apr. 21, 2026](https://astrologyapi.com/?s=2025)). |

### Two-Layer "Question Classifier + Specialized Calculator" Precedents

The two-layer pattern appears under several names: semantic routing, tool selection, agent orchestration, and active tool discovery. MCP-Zero's June 2025 paper explicitly proposes active tool request plus hierarchical semantic routing across an MCP tool inventory ([MCP-Zero summary, June 1, 2025](https://www.sciencestack.ai/paper/2506.01056v4)). Gemini CLI's implementation already has a discovery layer that registers configured MCP tools and an execution layer that wraps them as callable Gemini tools, but it does not discover arbitrary unconfigured servers ([Gemini CLI docs, accessed Apr. 21, 2026](https://google-gemini.github.io/gemini-cli/docs/tools/mcp-server.html)). OpenAI's Responses API similarly lists tools from specified remote MCP servers and then lets the model call them ([OpenAI connectors/API docs, accessed Apr. 21, 2026](https://platform.openai.com/docs/guides/tools-connectors-mcp?lang=javascript)).

Commercially, the first layer is often owned by the platform or host app: ChatGPT, Claude, Cursor, VS Code, Gemini CLI, and agent-infra startups decide which configured tools are visible and how calls are approved. YC's 2026 directory includes Orthogonal, which says "APIs weren't built for agents" and offers access to hundreds of APIs through MCP or SDK with key/billing abstraction, suggesting a commercial attempt at the router/distribution layer rather than a single domain calculator ([YC Infrastructure directory, accessed Apr. 21, 2026](https://www.ycombinator.com/companies/industry/infrastructure)).

### Astrology-Adjacent AI and Consumer Apps

Consumer astrology remains app-centric, not infrastructure-centric. Statista's public topic page says Co-Star had 30M registered users in July 2023, Nebula had approximately 3.9M worldwide downloads in 2023, Nebula had more than $7M U.S. in-app revenue in 2023, and CHANI had about $6.8M U.S. app revenue in that examined year; these figures are useful market evidence but the user-count and 2023 revenue figures are older than 12 months ([Statista topic page, accessed Apr. 21, 2026; 2023 figures older than 12 months](https://www.statista.com/topics/12325/horoscope-and-astrology-apps/)). Statista's June 24, 2025 page says U.S. horoscope-app revenue rankings for September 2024 included Chani and Co-Star, and notes that Co-Star integrated ChatGPT protocols in 2023 for paid in-app astrological readings; this is also partly older than 12 months for the underlying Co-Star integration ([Statista, June 24, 2025](https://www.statista.com/statistics/1451664/top-horoscope-apps-us-market-revenue/)).

Recent secondary astrology-app commentary describes Co-Star as AI/social, CHANI as human-written subscription content, Nebula as AI-powered, Sanctuary as blending AI-generated readings with live astrologers, and AstroTalk as a large Indian live-astrologer marketplace; because these are secondary/industry sources rather than audited filings, they should be used as directional market context rather than proof of business performance ([Lunar Guide Co-Star review, Feb. 23, 2026](https://www.lunarguideapp.com/blog/co-star-astrology-app-review-2025); [Aurae CHANI review, Apr. 2026](https://www.auraeastrology.com/blog/chani-app-review-2026-an-astrologers-honest-opinion); [TS2, Aug. 21, 2025](https://ts2.tech/en/astrology-just-got-an-ai-upgrade-inside-the-rise-of-ai-powered-horoscopes/); [OpenPR market release, Mar. 21, 2026](https://www.openpr.com/news/4433905/astrology-app-market-is-going-to-boom-major-giants-astrotalk)).

The most direct astrology infrastructure/API evidence found is AstrologyAPI, not Co-Star/Sanctuary/Nebula/CHANI/The Pattern. AstrologyAPI is already presenting "MCP Server" as a way to connect Claude, GPT, or any LLM directly to ephemeris data, which is very close to the founder's contemplated positioning ([AstrologyAPI, accessed Apr. 21, 2026](https://astrologyapi.com/?s=2025)).

I did not find solid public evidence in this pass that Co-Star, Sanctuary, Nebula, The Pattern, or CHANI has moved from consumer app to a broad external API/MCP infrastructure business. The public evidence points to consumer subscriptions, social features, human/AI interpretation, live astrologer marketplaces, and content ecosystems rather than developer-facing infrastructure.

### Open Issues or Uncertainties

- Directory activity metrics are not standardized, so "views," "installs," stars, and ratings cannot be compared across MCP marketplaces.
- AstrologyAPI's business metrics are vendor claims; no independent audit was found in this pass.
- There may be private B2B astrology APIs or white-label providers not discoverable through public search.
- Public evidence for "question classifier + specialized calculator" as a named commercial product is thinner than the founder's intuition; the pattern exists, but often inside platform/tool-routing layers.

### Implications for the Founder

- The astrology MCP/API opportunity is already visible to competitors; differentiation cannot be "first MCP astrology calculator."
- A focused, transparent, developer-friendly astrology calculation API can still compete if it is more accurate, easier to call, better documented, and better packaged for AI agents.
- Consumer astrology app incumbents validate demand but may not be ideal infrastructure competitors unless they open APIs.
- A "classifier + calculator" architecture is strategically plausible, but the first version should probably route within your own domain before trying to become a cross-domain router.
- Public credibility can come from open schemas, reproducible calculations, documented ephemeris/versioning, and clear comparison against existing astrology APIs.

## Cluster 7 - Investment Landscape

### Current-State Summary

The investment tailwind is real, but the money is flowing into **agent infrastructure**, **MCP tooling**, **sandboxing**, **connectors**, **browser/computer use**, **agent reliability**, and **universal API access** more than into narrow standalone MCP servers. Anthropic's December 2025 donation of MCP to the Linux Foundation's Agentic AI Foundation, co-founded with Block and OpenAI and backed by Google, Microsoft, AWS, Cloudflare, and Bloomberg, is a platform-level signal that the major players want shared standards for agentic AI infrastructure ([Anthropic, Dec. 9, 2025](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation?content=Dec2024EOYShips&medium=email&messageTypeId=140367&source=i_email)).

### Named Deals and Companies

Manufact, formerly `mcp-use`, is the clearest MCP-specific startup funding example found. VentureBeat reported on March 11, 2026 that Manufact raised $6.3M seed led by Peak XV, with participation from Liquid 2 Ventures, Ritual Capital, Pioneer Fund, Y Combinator, and angels; the article says the company emerged from YC Summer 2025 and is building infrastructure to help developers create and deploy MCP servers/apps for agents ([VentureBeat, Mar. 11, 2026](https://venturebeat.com/infrastructure/manufact-raises-usd6-3m-as-mcp-becomes-the-usb-c-for-ai-powering-chatgpt-and?lang=en)). YC's directory says Manufact developed `mcp-use`, claims 5M+ downloads and 9,000 GitHub stars, and says it is used by 20% of the US 500; these are YC/company profile claims rather than independently audited metrics ([YC Infrastructure directory, accessed Apr. 21, 2026](https://www.ycombinator.com/companies/industry/infrastructure)).

YC's 2025-2026 directories show many related companies:

- Orthogonal, YC P2026, says developers and agents get access to hundreds of APIs through MCP or SDK with no API key management and pay-as-you-go billing ([YC Infrastructure directory, accessed Apr. 21, 2026](https://www.ycombinator.com/companies/industry/infrastructure)).
- Castari, YC F2025, says it provides secure autoscaling sandboxes and handles tool/MCP connectors so developers can deploy agents faster ([YC Infrastructure directory, accessed Apr. 21, 2026](https://www.ycombinator.com/companies/industry/infrastructure)).
- Kernel, YC S2025, sells fast browser/computer-use infrastructure for agents and says it is backed by Accel & YC and trusted by Cash App, Rye, and 1,000+ others ([YC Infrastructure directory, accessed Apr. 21, 2026](https://www.ycombinator.com/companies/industry/infrastructure)).
- Dedalus Labs, YC S2025, describes a multi-model Agents SDK and Agents-as-a-Service endpoint callable by OpenClaw and Claude Code ([YC Infrastructure directory, accessed Apr. 21, 2026](https://www.ycombinator.com/companies/industry/infrastructure)).
- Metis, YC S2025, says it builds infrastructure enabling AI agents to perform complex tasks reliably in production ([YC Infrastructure directory, accessed Apr. 21, 2026](https://www.ycombinator.com/companies/industry/infrastructure)).
- AgentMail, YC S2025, offers email inbox APIs for AI agents, which is not MCP-specific but fits the "tools for agents" infrastructure theme ([YC Infrastructure directory, accessed Apr. 21, 2026](https://www.ycombinator.com/companies/industry/infrastructure)).

Business Insider reported that 70 of 144 startups in YC's Spring 2025 batch were building AI agents, and that YC invests $500,000 in every selected company; this is a strong accelerator-level signal even though the article is secondary reporting ([Business Insider, June 11, 2025](https://www.businessinsider.com/y-combinator-yc-demo-day-spring-ai-agent-startups-2025-6)).

### Investor Narratives and Hype Cycle

The public narrative is that agents need a standardized way to access tools, data, and actions. Anthropic's donation post frames MCP as an open, vendor-neutral standard for agentic AI and reports broad platform/infrastructure adoption ([Anthropic, Dec. 9, 2025](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation?content=Dec2024EOYShips&medium=email&messageTypeId=140367&source=i_email)). VentureBeat's Manufact coverage explicitly uses the "USB-C for AI" framing, but that phrase is a hype metaphor and should not be confused with proven market consolidation ([VentureBeat, Mar. 11, 2026](https://venturebeat.com/infrastructure/manufact-raises-usd6-3m-as-mcp-becomes-the-usb-c-for-ai-powering-chatgpt-and?lang=en)).

The hype cycle appears to be between early mainstream adoption and peak expectations. Evidence for rising adoption includes the Linux Foundation donation, major client support, the official registry, remote MCP support in OpenAI Responses API, Google Cloud database MCP work, and YC/VC funding into agent infrastructure ([Anthropic, Dec. 9, 2025](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation?content=Dec2024EOYShips&medium=email&messageTypeId=140367&source=i_email); [OpenAI, May 21, 2025](https://openai.com/index/new-tools-and-features-in-the-responses-api/); [Google Cloud, Apr. 22, 2025](https://cloud.google.com/blog/products/ai-machine-learning/mcp-toolbox-for-databases-now-supports-model-context-protocol); [MCP Registry blog, Sept. 8, 2025](https://blog.modelcontextprotocol.io/posts/2025-09-08-mcp-registry-preview/); [VentureBeat, Mar. 11, 2026](https://venturebeat.com/infrastructure/manufact-raises-usd6-3m-as-mcp-becomes-the-usb-c-for-ai-powering-chatgpt-and?lang=en)). Evidence for coming disillusionment includes security reports, fragmented distribution, immature monetization, and uneven tool-selection reliability ([TechRadar, Jan. 21, 2026](https://www.techradar.com/pro/security/anthropics-official-git-mcp-server-had-some-worrying-security-flaws-this-is-what-happened-next); [ITPro, Apr. 2026](https://www.itpro.com/security/ai-agents-using-anthropic-mcp-supply-chain-attacks-claim-researchers); [Zed docs, accessed Apr. 21, 2026](https://zed.dev/docs/ai/mcp)).

### Notable Acquisitions

No acquisition of an MCP-server-first company was found in this pass. This absence is itself relevant: the market is young enough that most public signals are launches, funding rounds, directories, and platform integrations rather than exits.

### Open Issues or Uncertainties

- I did not find enough named investor blog posts specifically about niche MCP servers; most investor-visible narratives are about AI agents and infrastructure.
- Deal data for private MCP/agent infrastructure companies is incomplete unless companies announce rounds.
- It is unclear whether the eventual value accrues to server publishers, client platforms, connector marketplaces, or hosted auth/billing layers.

### Implications for the Founder

- Investors will understand "agent infrastructure" more readily than "astrology MCP server."
- A niche calculation provider is probably too narrow for venture unless it becomes a broader category platform, data/API network, or high-margin vertical SaaS.
- Bootstrapping is plausible because the initial product is a wrapper around an existing engine and community.
- Funding becomes more plausible after evidence: paid API usage, cross-platform installs, partner demand, or a repeatable "calculation-as-tool" platform beyond astrology.
- Positioning should emphasize deterministic domain computation for agents, not astrology alone, while using astrology as the wedge.

## Cluster 8 - Strategic Implications for a Niche Calculation / Classification Provider

### Synthesis

The founder's core insight is directionally correct: general-purpose LLMs need specialized calculation and current/authoritative datasets for many domain questions, and MCP/skills are emerging as a standard way to expose those capabilities. The risk is mistaking protocol support for distribution. In April 2026, the winning pattern is not "publish an MCP server and wait"; it is "own a valuable calculation/data service, expose it through multiple agent surfaces, and create enough routing/discovery/install scaffolding that the right model calls it at the right time."

For AstroPrompt, the strongest near-term wedge is a deterministic astrology calculation API with:

- birth chart, transits, progressions, synastry, aspects, moon phases, void-of-course windows, and house-system choices;
- explicit input requirements and validation;
- structured JSON outputs with human-readable summaries;
- provenance fields such as ephemeris version, calculation engine/version, timezone/geocoding source, and house system;
- interpretation scaffolds for LLMs that separate calculated facts from interpretive text;
- a remote MCP server with OAuth/API-key auth;
- Claude skill / GPT / ChatGPT App-style wrappers that teach when and how to call the calculator.

### Option 1 - Open-Source + Paper Route

This route publishes the MCP server and maybe a subset of the calculation engine as open source, then publishes an arXiv-style paper describing the architecture, datasets, and evaluation. It can create credibility, attract developer contributors, and increase search/training visibility, but it is not a reliable distribution channel. Current MCP discovery evidence shows that clients route among configured tools; publishing a paper does not automatically place your tool in a user's ChatGPT/Claude/Cursor environment ([Cursor docs, accessed Apr. 21, 2026](https://docs.cursor.com/context/model-context-protocol); [OpenAI Docs MCP, accessed Apr. 21, 2026](https://platform.openai.com/docs/docs-mcp); [MCP-Zero summary, June 1, 2025](https://www.sciencestack.ai/paper/2506.01056v4)).

What has to be true for it to work: the open-source implementation must become trusted, developers must care about astrology calculation accuracy, and the paper must be cited or discovered by humans building agents. Realistic time-to-evidence: 4-8 weeks for GitHub stars/install feedback; 3-6 months for citations/community traction. Capital required: low. Incumbent risk: medium because API competitors can copy the wrapper; lower if your calculation quality and community trust are defensible.

### Option 2 - Marketplace Route

This route publishes to the official MCP Registry, Smithery, mcpservers.org, Apify-style catalogs if relevant, Cursor/VS Code install links, and any Claude/OpenAI connector directories available. It monetizes with a metered API behind the MCP wrapper. The official registry's preview and Smithery's marketplace/OAuth positioning make this feasible, but not sufficient ([MCP Registry blog, Sept. 8, 2025](https://blog.modelcontextprotocol.io/posts/2025-09-08-mcp-registry-preview/); [MCP Registry docs, accessed Apr. 21, 2026](https://modelcontextprotocol.io/registry/about); [Smithery docs, accessed Apr. 21, 2026](https://smithery.ai/docs)).

What has to be true for it to work: target users search for astrology/calculation tools in MCP directories, install friction is manageable, and a free tier converts to paid. Realistic time-to-evidence: 2-6 weeks for listing/install metrics; 1-3 months for paid conversion. Capital required: low to moderate, mostly docs, auth, billing, and support. Incumbent risk: high if platform directories favor larger generic API providers or first-party connectors.

### Option 3 - Skill-First Route

This route packages AstroPrompt as a Claude skill, custom GPT/action, ChatGPT connector/app, and possibly Gemini/Chrome prompt workflow. It emphasizes user-facing workflow quality and interpretation discipline before trying to be a general MCP infrastructure product. Anthropic Skills and OpenAI GPTs/Apps provide concrete surfaces for this route ([Anthropic, Oct. 16, 2025](https://www.anthropic.com/news/skills?t=n); [Claude Code Skills docs, accessed Apr. 21, 2026](https://docs.claude.com/en/docs/claude-code/skills); [OpenAI GPTs Help, updated Apr. 2026](https://help.openai.com/en/articles/8555535-gpts-chatgpt-enterprise-version); [OpenAI Apps, Oct. 6, 2025](https://openai.com/index/introducing-apps-in-chatgpt/)).

What has to be true for it to work: users already ask astrology questions in Claude/ChatGPT, platform skill/app discovery surfaces are accessible, and the skill can call a deterministic backend when needed. Realistic time-to-evidence: 2-4 weeks for private beta; 1-2 months for engagement/retention. Capital required: low. Incumbent risk: high on consumer distribution because OpenAI/Anthropic can change app/skill policies, and astrology app incumbents can launch their own wrappers.

### Option 4 - Two-Layer Route

This route builds a layer-1 question classifier/router MCP server that detects whether a question needs astrology calculation, then routes to layer-2 specialized calculators. A narrow version routes within astrology: natal, transit, synastry, electional, horary-style caveats, moon calendar, etc. A broad version becomes a cross-domain router for any specialized calculator category. MCP-Zero and current client tool-discovery patterns support the technical plausibility of routing, but product evidence for independent router businesses is still thin ([MCP-Zero summary, June 1, 2025](https://www.sciencestack.ai/paper/2506.01056v4); [Gemini CLI docs, accessed Apr. 21, 2026](https://google-gemini.github.io/gemini-cli/docs/tools/mcp-server.html); [YC Orthogonal profile, accessed Apr. 21, 2026](https://www.ycombinator.com/companies/industry/infrastructure)).

What has to be true for it to work: the router improves tool selection more than platform-native routing, the router has access to useful downstream tools, and users trust it with domain classification. Realistic time-to-evidence: 1-2 months for an astrology-only router; 6-12 months for cross-domain ambitions. Capital required: moderate if cross-domain; low if astrology-only. Incumbent risk: very high because platforms and marketplaces are natural owners of global routing.

### Option 5 - Gate + Partnership Route

This route keeps the API proprietary, exposes controlled MCP/skill wrappers, and pursues direct partnerships with AI platforms, astrology apps, prosumer tools, media properties, or wellness communities. It is slower but more defensible if the data/calculation layer is genuinely better and the founder has warm niche access. Existing consumer astrology apps validate demand, while AstrologyAPI validates API/MCP infrastructure positioning ([Statista topic page, accessed Apr. 21, 2026](https://www.statista.com/topics/12325/horoscope-and-astrology-apps/); [AstrologyAPI, accessed Apr. 21, 2026](https://astrologyapi.com/?s=2025)).

What has to be true for it to work: partners need accurate astrology calculations inside their own AI experiences, and the founder can prove quality, latency, safety, and commercial terms. Realistic time-to-evidence: 2-4 weeks for warm conversations; 2-6 months for first integration. Capital required: low to moderate, mostly API polish and business development. Incumbent risk: medium because partners may prefer larger vendors, but specialization and trust can matter.

### Additional Option 6 - API-First + MCP/Skill Wrappers

This is the pragmatic default. Build the paid API as the durable product, then treat MCP, skills, GPT actions, ChatGPT apps, and registries as channels. This matches how Browserbase, Equidam, AstrologyAPI, Apify actors, and Google Cloud database tooling appear to monetize: the MCP interface is a way in, not the whole business ([Browserbase, July 22, 2025](https://www.browserbase.com/changelog/browserbase-mcp); [Equidam, Aug. 8, 2025](https://www.equidam.com/equidam-mcp-startup-valuation-ai/); [AstrologyAPI, accessed Apr. 21, 2026](https://astrologyapi.com/?s=2025); [Google Cloud, Apr. 22, 2025](https://cloud.google.com/blog/products/ai-machine-learning/mcp-toolbox-for-databases-now-supports-model-context-protocol)).

What has to be true for it to work: the API solves a painful enough accuracy/workflow problem, and wrappers reduce acquisition friction. Realistic time-to-evidence: 2-8 weeks for beta API usage; 3 months for paid patterns. Capital required: low. Incumbent risk: medium; incumbents can wrap their APIs, but a focused founder can move faster in a niche.

### Additional Option 7 - Vertical App + Infrastructure Hybrid

This route keeps AstroPrompt as a user-facing app while exposing the calculation layer through API/MCP to selected developers. It avoids overcommitting to immature discovery surfaces and preserves a direct user feedback loop. Consumer astrology market data shows subscription and app revenue opportunities, but public evidence does not show most astrology app incumbents becoming infrastructure providers ([Statista, June 24, 2025](https://www.statista.com/statistics/1451664/top-horoscope-apps-us-market-revenue/); [Statista topic page, accessed Apr. 21, 2026](https://www.statista.com/topics/12325/horoscope-and-astrology-apps/)).

What has to be true for it to work: the app generates demand/feedback, and a subset of users/developers want programmatic access. Realistic time-to-evidence: immediate from current users; 1-3 months for API demand. Capital required: low. Incumbent risk: medium; consumer incumbents are strong, but infrastructure remains less crowded.

### Recommended Strategic Path

The recommended path is **Option 6 with pieces of Options 2, 3, and 7**:

1. Productize the existing calculation layer as a versioned API with strict schemas, test fixtures, and provenance.
2. Build a remote MCP server with 5-8 high-value tools rather than a large unfocused tool list.
3. Build one Claude skill and one custom GPT/action or ChatGPT custom connector that call the API/MCP server.
4. Publish metadata to the official MCP Registry and Smithery, and create install snippets for Claude Code, Cursor, VS Code, and OpenAI API.
5. Run a 30-day evidence sprint with warm users: measure install success, questions asked, missing-input rates, calculation calls, hallucination reduction, paid conversion intent, and repeat usage.
6. Only after evidence, decide whether to open-source the wrapper, publish a paper, or pursue partnerships.

## Strategic Options Comparison Table

| Option | Preconditions | Time-to-evidence | Capital required | Incumbent risk | Upside if it works | Recommended next action |
| --- | --- | --- | --- | --- | --- | --- |
| Open-source + paper | Developers care about reproducible astrology calculations; paper/docs attract human attention; open parts do not destroy monetization. | 4-8 weeks for repo feedback; 3-6 months for citations/community. | Low. | Medium. Competitors can copy wrapper patterns. | Credibility, community, search visibility, possible standards influence. | Open-source only the MCP wrapper/schema/test fixtures first; keep paid hosted API proprietary. |
| Marketplace route | MCP users search for astrology/calculation tools; registries send real installs; metered API conversion works. | 2-6 weeks for installs; 1-3 months for paid conversion. | Low to moderate. | High. Larger API providers and platform-preferred connectors may crowd listings. | Low-cost acquisition across Claude/Cursor/VS Code/OpenAI ecosystems. | Publish to official registry + Smithery with install snippets and track referral/install source. |
| Skill-first route | Users ask astrology questions inside Claude/ChatGPT; skill/app surfaces are accessible; skill reliably calls backend. | 2-4 weeks for beta; 1-2 months for retention. | Low. | High. Platform UX and policy can change. | Better user experience than raw MCP; easier for non-developers. | Ship a Claude skill and custom GPT/action around the same API. |
| Two-layer route | Router materially improves tool selection; enough downstream calculators exist; users trust classification. | 1-2 months for astrology-only; 6-12 months for cross-domain. | Low for astrology-only; moderate/high for cross-domain. | Very high. Platforms and marketplaces own natural routing layer. | Category leadership if the router becomes the default "domain calculation broker." | Start with internal astrology router only; do not pursue cross-domain routing until usage proves demand. |
| Gate + partnership | Partners need accurate calculations; API quality is provable; founder can reach decision-makers. | 2-4 weeks for conversations; 2-6 months for first integration. | Low to moderate. | Medium. Partners may prefer larger vendors. | Fewer users, higher-value distribution, defensible relationships. | Prepare partner demo: "LLM answer with and without grounded calculations" plus latency/pricing. |
| API-first + wrappers | API solves accuracy/workflow pain; wrappers drive acquisition; billing works independent of platform revenue share. | 2-8 weeks for beta API; 3 months for paid signal. | Low. | Medium. | Durable product not tied to one platform; wrappers become channels. | Make this the default 90-day plan. |
| Vertical app + infrastructure hybrid | Existing users validate workflows; API demand emerges from power users/builders. | Immediate app feedback; 1-3 months for API demand. | Low. | Medium. | Keeps direct user relationship while testing infrastructure. | Add "export/use via agent" workflows to current AstroPrompt users and watch pull. |

## What I Don't Know

- I did not find audited, MCP-specific revenue numbers for paid MCP servers; most public evidence is underlying SaaS traction, platform claims, or directory metrics.
- I did not find a public Anthropic or OpenAI revenue-share program that clearly pays third-party MCP server or skill publishers at scale as of April 21, 2026.
- I did not find strong evidence that publishing an arXiv paper causes future frontier models to route to a specific MCP server; the mechanism remains speculative.
- I did not find reliable public evidence that Co-Star, Sanctuary, Nebula, The Pattern, or CHANI has become a developer-facing astrology infrastructure/API company.
- I did not verify AstrologyAPI's self-reported 100M+ daily API calls or 3,000+ active businesses with independent sources.
- I did not find enough public examples of commercial "question classifier + specialized calculator" products where the classifier is sold independently of the host platform.
- I did not evaluate legal/compliance exposure for astrology advice, medical-style disclaimers, consumer protection, or app-store policy; those could matter if the service makes predictive or wellness-adjacent claims.
- I did not test actual install/use flows for Claude Desktop, Claude Code, Cursor, VS Code, ChatGPT custom connectors, or Gemini CLI against a prototype astrology MCP server; that should be the next evidence-gathering step.
