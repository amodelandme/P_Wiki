---
title: "Search tools for context engineering"
source: "https://www.elastic.co/search-labs/blog/search-tools-context-engineering"
author:
  - "[[Leonie Monigatti]]"
published: 2026-03-24
created: 2026-05-16
description: "Learn what context-retrieval tools exist for context engineering, how they work, and their trade-offs."
tags:
  - "clippings"
---
The most important tools an agent has are the search tools it can use to build its own context. Recent posts by [LlamaIndex](https://www.llamaindex.ai/blog/files-are-all-you-need) and [LangChain](https://x.com/hwchase17/status/2011814697889316930) have sparked a discussion: *Are a shell tool and a filesystem all an agent needs for [context engineering](https://www.elastic.co/search-labs/blog/context-engineering-overview)?* Unfortunately, the discussion quickly drifted to the wrong focus: filesystem versus database.

This post refocuses on the question,*What are the right search interfaces an agent needs to build its own context?* It first covers the trade-offs between shell tools and dedicated database tools. From there, it offers a practical framework for finding the right interfaces for your agent's needs.

## What does "building context" actually mean for an agent?

In early [retrieval augmented generation (RAG) pipelines](https://www.elastic.co/what-is/retrieval-augmented-generation), the developer engineered a fixed retrieval pipeline, and the [large language model](https://www.elastic.co/what-is/large-language-models) (LLM) was a passive recipient of the context. This was a fundamental limitation: Context was retrieved on every , whether or not it was needed, with no check that it actually helped.

With the shift to agentic [RAG](https://www.elastic.co/search-labs/blog/retrieval-augmented-generation-rag "Learn more about retrieval augmented generation (RAG)"), the agents now have access to a set of search tools to build their own context. For example, both Claude Code \[1\] and Cursor \[2\] let the agent choose between different search tools and even combine them for chained queries, depending on what the task actually requires.

## What search interfaces exist for context engineering?

Context can live in different locations, such as on the web, in a local filesystem, or in a database. An agent can interact with each of these out-of-context data sources through different tools:

- **Shell tools** can execute shell commands and have access to the local filesystem. Some examples of built-in shell tools are [Claude API's bash tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/bash-tool), [OpenClaw's exec tool](https://docs.openclaw.ai/tools/exec), and [LangChain's shell tool](https://docs.langchain.com/oss/python/integrations/tools/bash).
- **Dedicated database tools,** such as tools from a Model Context Protocol ([MCP](https://www.elastic.co/search-labs/blog/mcp-current-state#what-is-model-context-protocol-\(mcp\)?)) server (for example, the [Elastic Agent Builder MCP server)](https://www.elastic.co/docs/explore-analyze/ai-features/agent-builder/mcp-server) or custom tools (for example, `run_esql(query)` or `db_list_index()`), can query databases.
- **Dedicated file search tools** can search and read local (or uploaded) files (without full shell access). Some examples of built-in file search tools are [Gemini API’s File Search Tool](https://ai.google.dev/gemini-api/docs/file-search) or [OpenAI’s File Search Tool](https://developers.openai.com/api/docs/guides/tools-file-search).
- **Web search tools** can retrieve information from the web.
- **Memory tools** store and recall from long-term memory (regardless of how it’s stored).

![Diagram showing how an agent uses different context‑retrieval tools to access local files, proprietary data, the web, and long‑term memory.](https://www.elastic.co/search-labs/_next/image?url=https%3A%2F%2Fcdn.sanity.io%2Fimages%2Fme0ej585%2Fsearch-labs-import-testing%2F115f20c8ded259e508f51524b2c06bdc702d70ab-1999x1050.png&w=3840&q=75)

Diagram showing how an agent uses different context‑retrieval tools to access local files, proprietary data, the web, and long‑term memory.

As you can see, the shell tool is versatile and can be used to retrieve context from different data sources, including:

- **Filesystem:** The agent explores the directory structure (ls, find), searches for relevant content (grep, cat), and repeats until it has built sufficient context.
- **Database:** The agent can use database command line interface (CLI) tools (for example, [`elasticsearch-sql-cli`](https://www.elastic.co/docs/reference/query-languages/sql/sql-cli)), call HTTP APIs via curl, or run scripts, which is especially useful in combination with [agent skills](https://www.elastic.co/search-labs/blog/agent-skills-elastic), which are reusable, documented examples injected into the agent's context to guide correct tool usage (for example, [Elastic Agent Skills for Elasticsearch](https://github.com/elastic/agent-skills)).
- **Web:** The agent can execute web searches via a curl command through a search provider’s API.

However, the shell tool provides direct system access and therefore requires safety measures, such as running in an isolated sandbox environment and logging all executed commands.

## When to use which search interfaces

The right search interface depends on your data, your query patterns, and your use case. This section serves as a practical starting point.

### Filesystems aren’t making databases obsolete

The filesystems-versus-databases discussion is not about the storage layer. For example, LangChain explains that [its memory system](https://x.com/hwchase17/status/2011814697889316930) doesn’t actually store memory in a real filesystem. Instead, it stores memory in a database and *represents* it as a set of files to the agent \[3\].

Filesystems are a natural fit for file-native use cases, such as coding agents. They also work well as a temporary scratch pad or working memory and for single-user or single-agent scenarios where concurrency isn't a concern. In these cases, a physical filesystem or representing the data as a filesystem gives you flexibility before committing to a purpose-built interface.

But filesystem storage has real downsides, such as weak concurrency, manual schema enforcement, and atomic transactions. These become more apparent when your application needs to scale or move to a multi-agent scenario. Anyone who ignores these downsides is doomed to [painfully reinvent worse databases](https://dx.tips/oops-database) without the decades of engineering behind transaction safety or access control that production databases already provide. Additionally, in most enterprise contexts, you don't choose whether to use a database since it's already there, storing business-critical data.

### Shell tool + filesystem

A shell tool is the natural starting point for filesystem search. Currently, coding agents are driving a lot of progress in the field. Because they work with code in local files, they’re naturally file-heavy use cases. Therefore, LLMs are fine-tuned in the post-training stage for coding tasks. That’s why many LLMs are not only good at writing code but also at using shell commands and navigating filesystems.

Using a shell tool with built-in CLIs, like `ls` and `grep`, to find files is effective. With grep, a query like "Find all files that import `matplotlib` " is fast, precise, and cheap. But when the agent needs to handle conceptual queries, such as "How does our app handle failed authentication?", pattern matching with grep can hit a ceiling quickly. Several alternatives that bring [semantic search](https://www.elastic.co/search-labs/blog/introduction-to-vector-search "Learn more about vector search") capabilities to the command line have emerged to fill this gap, including [`jina-grep`](https://github.com/jina-ai/jina-grep-cli).

However, grep and many of its semantic search alternatives run in O(n) over the . For use cases over codebases, this might be fine. However, if your data grows, will become noticeable. In this case, an indexed datastore becomes necessary to maintain performance.

### Shell tool + database

Another way to add more search capabilities, such as semantic or [hybrid search](https://www.elastic.co/search-labs/blog/hybrid-search-elasticsearch "Learn more about hybrid search"), over your data is to store it in a database, as Cursor does, for example. Additionally, when data requires complex relational joins or aggregations, a database interface is nonnegotiable.

When the data lives in a database rather than on the filesystem, a shell tool can serve as a lightweight database interface for certain use cases. If your queries are simple enough for a CLI or a curl call, a dedicated database tool may add unnecessary complexity.

This approach is also suitable in early exploration stages, when you don't yet know what query patterns your agent will actually develop. In this case, Agent Skills can give the agent enough structure to query correctly without committing to a purpose-built tool. However, when the agent requires many iterations to figure out the right way to query the database for repeated tasks, the overhead of using a shell tool as the interface no longer justifies the simplicity benefit of avoiding an extra tool.

### Dedicated database tool

Especially when repeated query patterns are structured or analytical, dedicated database tools become necessary. A [blog post from Vercel and Braintrust](https://vercel.com/blog/testing-if-bash-is-all-you-need) compared agents with different sets of search tools for real-world retrieval tasks over semi-structured data, such as customer support tickets and sales call transcripts (for example, “How many open issues mention 'security'?" or "Find issues where someone reported a bug and later someone submitted a PR claiming to fix it?") \[4\].

Agents with dedicated database tools used fewer tokens, were faster, and made fewer mistakes than agents with only a shell tool and filesystem. The lesson is that direct database tools are the right choice when the query requires analytical reasoning over semi-structured data.

### Combining search interfaces

No single search interface handles every query well. For example, Cursor combines shell tools (for searches via grep) and semantic search tools and lets the agent select the right tool based on the user’s prompt. They report that the agent chooses grep for matching specific symbols or strings, semantic search for conceptual or behavior questions, and both for exploratory tasks.

The Vercel experiment reports the same: Its hybrid agent with access to both a shell tool and a dedicated database tool achieved the best performance out of all tested agents by first using the dedicated database tools and then verifying the results by grepping through the filesystem. However, this approach uses more tokens and time for reasoning about tool choice and verification.

The pattern across both examples is the same: Composition [beats](https://www.elastic.co/beats) any single interface, but composition comes at the trade-off of added cost and latency.

## Practical recommendations for finding the right set of tools

The right set of search interfaces is small, purposeful, and specific to your agent's actual query patterns. The current best practice is to have an agent with as few tools as possible instead of having an agent with hundreds of MCP tools. This is because the downside of exposing all possible tools up front is that it bloats the context window and confuses the agent about which tool to actually use. For example, Claude Code reportedly only has about 20 tools.

Instead, the idea of progressive disclosure is to start with a minimal set of tools and let the agent discover additional capabilities only when needed. Research from Anthropic \[5\] and Cursor \[6\] has shown that this approach yields a token savings between 47%–85%. Claude Code, for example, implements this directly, allowing the agent to incrementally discover how to query an API or a database, without that knowledge consuming context on every LLM call.

Once you’re familiar with the agent's query patterns, you can revisit the set of search tools that the agent has access to by default. A useful way to think about this trade-off is the ["low floor, high ceiling" principle](https://www.elastic.co/search-labs/blog/database-retrieval-tools-context-engineering#building-the-right-database-retrieval-tools-%5C\(%E2%80%9Clow-floor,-high-ceiling%E2%80%9D%5C) for deciding which tools make the cut. High-ceiling tools don't limit the agent's potential. For example, a versatile shell tool lets the agent write full database queries, including ambiguous ones, but at the cost of reasoning overhead, higher latency, and lower reliability.

Low-floor tools are the opposite. They’re specialized tools that wrap specific queries and are immediately accessible to the agent with minimal reasoning overhead, producing lower cost and higher reliability. But they need upfront engineering, can't cover every possible query, and can make it harder for the agent to choose the right tool.

Think of each tool on a spectrum: Low-floor tools are easy for the agent to use correctly but narrow in scope. High-ceiling tools are versatile but demand more reasoning to use well.

![Diagram comparing three agent‑design approaches (high floor/high ceiling, low floor/low ceiling, and low floor/high ceiling), showing how different tool strategies affect how agents handle ambiguous, versatile, and predictable queries.](https://www.elastic.co/search-labs/_next/image?url=https%3A%2F%2Fcdn.sanity.io%2Fimages%2Fme0ej585%2Fsearch-labs-import-testing%2Fe6d1b973be4b0a0a25c99c74f02a47e98395a3f7-1200x630.png&w=3840&q=75)

Diagram comparing three agent‑design approaches (high floor/high ceiling, low floor/low ceiling, and low floor/high ceiling), showing how different tool strategies affect how agents handle ambiguous, versatile, and predictable queries.

Most agents need a mix of different search tools. But each tool needs to earn its addition. We recommend starting with an all-purpose search tool (for example a `search_database()` tool or a shell tool). Then reuse the command logs you're already keeping for security purposes to track what your agent actually does, including tool calls, retries, and number of calls per user query. And, when you see a query pattern repeating or failing, that's the signal to build a purpose-built tool for it.

## Summary

The filesystem-versus-database debate is distracting from the actual question that engineers need to be asking: *What are the right search interfaces an agent needs to build its own context?* The answer is most likely, *Not a single one*.

A shell tool is a versatile tool to interact with different out-of-context sources and thus a good starting point. But it’s less efficient and accurate for use cases with structured analytical queries than dedicated database tools.

The goal is to find the minimal set of search tools that handles your agent's actual query patterns well. Start with a shell tool, and log what your agent actually does. When you see a query pattern repeating and failing, it’s time to engineer specialized tools.

## References

1\. Thariq (Anthropic). [Lessons from Building Claude Code: Seeing like an Agent](https://x.com/trq212/status/2027463795355095314) (2026).

2\. Cursor: Documentation. [Semantic & agentic search](https://cursor.com/docs/agent/tools/search) (2026).

3\. Harrison Chase (LangChain). [How we built Agent Builder’s memory system](https://x.com/hwchase17/status/2011814697889316930) (2026).

4\. Ankur Goyal (Braintrust) and Andrew Qu (Vercel). [Testing if "bash is all you need"](https://vercel.com/blog/testing-if-bash-is-all-you-need) (2026).

5\. Anthropic. [Introducing advanced tool use on the Claude Developer Platform](https://www.anthropic.com/engineering/advanced-tool-use) (2025).

6\. Cursor. [Dynamic context discovery](https://cursor.com/blog/dynamic-context-discovery) (2026).