---
created: 2026-08-09
last_edited: 2026-08-09
tags:
  - ai
connections: []
ai_generated: false
human_approved: false
category:
  - Knowledge Base
  - AI
  - Engineering
---
## Ecosystem unlocks potential
Like the IPhone in 2007 lacked the AppStore and die web in early 90s lacked TLS/SSL - arising ecosystems will
transform everything all over again. Tool libraries and agent portfolios will the key drivers of AI creeping
into real-world applications.
## Standardization as a signal for maturity and efficiency
As an engineer, I know that standardization is the key for unlocking efficient development. Game recognizes
game, in a sense, so I intuitively switched into this career when the MCP standard was crystalizing in March
2025. Also, making private information accessible to the AI client in minutes, not hours or day, is just
awesome.
## The N×M Integration Problem
Without MCP, developers must create custom integrations for each data source with each AI model/framework they
want to use. If they switch from GPT to Claude, or from a custom solution to LangChain, they must reimplement
all these integrations. What is the Model Context Protocol
(
MCP
)
?
The "just use APIs" approach ignores this exponential complexity. As the number of AI applications (N) and the
variety of tools and data sources (M) increase, the complexity of creating custom integrations for each
combination becomes unmanageable.
## The Subtle But Important Differences
Runtime Discovery vs. Build-time DeclarationOne major difference is when and how tools are defined. In classic
function calling, all possible functions must be declared upfront (at design time) in the prompt or through
the API. The model can only call those fixed functions. In MCP, tools are discoverable at runtime – the AI can
query what's available and even get new tools mid-session.With libraries, even if they're standardized, you
still need to:Import them into your applicationDeclare which functions are available to the modelRestart your
application to add new toolsWith MCP, new tools can be added dynamically without application restarts.2.
Process Isolation and SecurityThis is actually where your library approach faces real challenges. With shared
libraries:All tools run in the same process as your AI applicationAuthentication credentials are shared across
toolsOne misbehaving tool can crash your entire applicationSecurity permissions are all-or-nothingWith MCP,
your existing Claude interface could connect to your company's MCP server, giving you database access through
the interface you already use daily. When you're done, you can switch back to standard Claude's flow or
connect to another MCP server for different functionality — all without changing applications.  (Hacker
News)3. Cross-Application SharingHere's where your library idea hits a wall. You said "others could provide it
to their AI the same way" - but this assumes everyone is using the same programming language, runtime, and
application architecture.Since MCP is an open standard, any AI client (Claude, other LLM chatbots or
open-source LLMs) can use any MCP server. This means developers and companies can mix-and-match – e.g. use
Anthropic's Claude for some tasks, switch to an open-source LLM later – and their MCP-based integrations
remain intact.With libraries:Python tools can't be used by JavaScript applicationsDesktop applications can't
share tools with web applicationsDifferent AI frameworks require different integration patterns

