---
title: I Was Burning Through Claude Code’s Weekly Limit in 3 Days. Here’s How I Fixed It.
source: https://medium.com/@kunalbhardwaj598/i-was-burning-through-claude-codes-weekly-limit-in-3-days-here-s-how-i-fixed-it-0344c555abda
author:
  - "[[Kunalbhardwaj]]"
published: 2026-05-02
created: 2026-05-03
description: I build drone guidance systems for a living. Ground control stations, onboard computers, MAVLink protocols, gimbal control loops — real-time
tags:
  - clippings
  - Claude
  - equality
  - Agentic
  - contextwindow
---
I build drone guidance systems for a living. Ground control stations, onboard computers, MAVLink protocols, gimbal control loops — real-time systems where bugs mean a 30–100kg UAVs (Multicopters,  
VTOLs and Fixed Wing) crashes into something expensive.

Claude Code became my primary development environment about six months ago. I’m on the Pro plan. And every single week, without fail, I’d hit my token limit by Wednesday.

Not because I was being wasteful. Because the work genuinely required it — reading 800-line Python files to understand thread interactions, generating test harnesses, updating documentation after every feature session. Normal engineering work, but Claude was eating through my allocation like it was nothing.

I tried the standard tricks. Compact after every task. Use Sonnet for simple edits. Write tighter prompts. Didn’t matter. By midweek, I’d get the “you’ve reached your limit” message and my momentum would die.

So I did something different. I gave Claude a coworker.

**The Realization That Changed Everything**

One night, frustrated after hitting limits at 2 PM on a Tuesday, I sat down and actually read Claude Code’s \[tools reference documentation\]([https://code.claude.com/docs/en/tools-reference](https://code.claude.com/docs/en/tools-reference)). Not skimmed — read.

And I noticed something obvious that I’d been ignoring: **Claude Code’s Bash tool can run any command on your PATH.**

Any command. Any script. Anything that takes stdin and prints to stdout.

> That means Claude can call external APIs. Not through some plugin system or MCP server or extension marketplace. Just… a Python script in \`~/bin/\` that Claude runs via Bash.

The second realization: **most of what Claude does for me isn’t thinking. It’s I/O.**

Reading large files to answer a question about one function. Generating a test file that’s 80% boilerplate. Rewriting documentation after a conversation. These tasks burn thousands of tokens but require almost zero reasoning. Claude’s intelligence is overkill for them.

What if I could route these tasks to something cheaper?

**The Architecture: 60 Lines of Python, Zero Overhead**

I picked Kimi K2.5 (Moonshot AI) as my worker model. Why?

1. OpenAI-compatible API — the \`openai\` Python package works with zero changes, just swap the \`base\_url\`
2. 128K context window — can ingest my entire GCS codebase in one call  
	\- Costs roughly 1/100th of what equivalent Claude work would cost on my Pro plan
3. Has a “thinking” mode (internal chain-of-thought) that actually produces good technical analysis

But honestly, the specific model doesn’t matter. DeepSeek, Qwen, Gemini Flash — anything cheap with a long context window works. The pattern is what matters.

I wrote two CLI tools. Total implementation: about 60 lines each.

## Tool 1: \`ask-kimi\` — The Bulk Reader

This is for when Claude would otherwise read multiple large files into its context window just to answer one question.

```c
#!/path/to/venv/bin/python3
"""Delegate bulk reading to Kimi K2.5."""
import argparse, os, pathlib
from openai import OpenAI

client = OpenAI(
    api_key=os.environ["MOONSHOT_API_KEY"],
    base_url="https://api.moonshot.ai/v1",
)

# Read the files, pack them into a corpus
docs = []
for path in args.paths:
    content = pathlib.Path(path).read_text()
    docs.append(f"<file path='{path}'>\n{content}\n</file>")
corpus = "\n\n".join(docs)

# Ask Kimi to analyze them
resp = client.chat.completions.create(
    model="kimi-k2.5",
    messages=[
        {"role": "system", "content": "You are a precise code analyst..."},
        {"role": "user", "content": f"<corpus>\n{corpus}\n</corpus>"},
        {"role": "user", "content": args.question},
    ],
    max_tokens=8192,
)
print(resp.choices[0].message.content)
```

When Claude needs to understand something spread across multiple files, instead of reading them all (let’s say 5 files × 400 lines = ~8,000 tokens burned), it runs:

```c
ask-kimi --paths gcs_main.py gimbal_control.py network.py \
         --question "What IP addresses and ports are used for video streaming?"
```

Kimi reads all three files, produces a 300-word structured summary. Claude reads the summary (~400 tokens) and has its answer.

> **Before: 8,000 tokens. After: 400 tokens. Same information.**

## Tool 2: \`kimi-write\` — The Boilerplate Generator

For test files, config scaffolding, documentation — anything where Claude would spend expensive output tokens generating predictable text:

```c
kimi-write --spec "pytest test file for the MAVLink heartbeat parser" \
           --context src/mavlink_parser.py \
           --target tests/test_mavlink_parser.py
```

Kimi reads the reference file, generates the entire test file, writes it to disk. Claude then reviews what was generated and makes surgical edits — only spending tokens on the 5% that needs its intelligence.

**The Part Nobody Tells You: Making Claude Actually Use the Tools**

Here’s where most people would stop. “Cool, I have two scripts.” But Claude won’t use them unless you tell it to.

Claude Code reads a file called \`CLAUDE.md\` in your project root at the start of every session. It’s essentially Claude’s instruction manual for your project. And this is where the routing logic lives:

```c
## Kimi K2.5 Delegation Tools (Token Saving)

### ask-kimi — bulk reading
For reading files >400 lines, or when you'd otherwise read 3+ files:
  ask-kimi --paths <file1> <file2>... --question "<question>"
Returns a structured summary. Use that instead of reading files yourself.

### kimi-write — boilerplate generation
For tests, config files, docstrings, or repetitive patterns:
  kimi-write --spec "<what>" --context <reference> --target <output>
Then review the output and edit only what needs fixing.

### When NOT to delegate
- Tasks under ~2000 tokens (delegation overhead isn't worth it)
- Architectural decisions, debugging, safety-critical code
- Anything requiring careful reasoning
- When exact line numbers are needed for editing
```

That last section — “When NOT to delegate” — is critical. Without it, Claude would try to route everything to Kimi, including stuff where you genuinely need Claude’s reasoning. The boundary has to be explicit:

> Claude = thinking. Kimi = I/O.

Debugging a race condition between my camera thread and MAVLink thread? That’s Claude. Reading 5 files to understand which ports are in use? That’s Kimi. Writing a new test file matching existing patterns? That’s Kimi. Deciding whether a guidance calculation is numerically stable? Claude.

Once I put these rules in \`CLAUDE.md\`, Claude started self-routing without any manual intervention. I’d ask a question about my codebase and watch it reach for \`ask-kimi\` instead of reading the files itself. No friction. No extra prompting. It just… works.

**The Documentation Pipeline: The Biggest Win**

This was the unexpected multiplier.

After every feature session, I used to update my project docs in Obsidian. Claude would re-read the conversation context, re-read the existing docs, then write prose updates. Easily 5,000+ tokens per doc update — and I was doing this multiple times per day.

## Get Kunalbhardwaj’s stories in your inbox

Join Medium for free to get updates from this writer.

I built a third tool: \`extract-chat\`. It takes Claude Code’s JSONL session transcript and strips it down to just the human-readable conversation text (no tool calls, no system prompts, no binary data).

Now the documentation workflow is:

```c
# 1. Extract clean conversation text
extract-chat ~/.claude/projects/my-project/session.jsonl -o /tmp/chat.txt

# 2. Kimi reads the chat + existing docs, produces update suggestions
ask-kimi --paths /tmp/chat.txt docs/architecture.md \
         --question "Read the chat. What doc updates are needed? Give exact edits."

# 3. Claude applies Kimi's suggestions (tiny token cost)
```

Claude’s total token cost for a documentation update went from ~5,000 tokens to ~200 tokens. Twenty-five times less. And the documentation quality is the same because Kimi has the full conversation context and the full existing doc.

Black and Teal Modern Web Developer PresentationI put this in \`CLAUDE.md\` as a mandatory rule:

```c
### Documentation workflow (MANDATORY)
**NEVER write documentation directly. Always delegate to kimi.**
```

Claude follows it. Every time. No exceptions.

**Things I Learned the Hard Way**

**Kimi K2.5 is a “thinking” model — and it’ll eat your budget silently**

My first version used *\`max\_tokens=2048\`*. Kimi would use 1,500 tokens for internal reasoning (invisible to you) and have 548 tokens left for the actual answer. The response came back **completely empty**. No error, just… nothing.

The fix: always set *\`max\_tokens\`* high enough to cover both reasoning + answer. I use 8192 for reading tasks and 16384 for generation tasks. Kimi’s thinking tokens are cheap, but you have to budget for them.

**Corpus ordering enables cache hits**

Moonshot (like Anthropic) has prefix caching. If the first N tokens of your request match a previous request, they’re served from cache at 25% of the normal cost.

> So I always put the files first, question last:

```c
messages = [
    {"role": "user", "content": f"<corpus>{corpus}</corpus>"},
    {"role": "user", "content": question},  # only this changes between calls
]
```

Ask three questions about the same files? The first call pays full price, the next two are 75% cheaper because the corpus prefix is cached.

**Don’t delegate reasoning**

I tried routing debugging to Kimi early on. Bad move. It found surface-level issues but missed the subtle thread-safety bug that was actually causing my problem. Claude spotted it in 30 seconds once I gave it the right context.

The value of Claude isn’t reading files. Any model can do that. Claude’s value is connecting dots, spotting implications, understanding intent. Keep the thinking on Claude, route the paperwork elsewhere.

**The Numbers**

After three weeks of running this setup on real engineering work:

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*3JwOUdCXFwnc2ly7Z1SY8Q.png)

That’s not a typo. Thirty-eight cents. For three weeks of daily engineering work. And I haven’t hit my Claude Pro limit once since setting this up.

**You Don’t Need My Specific Stack**

The pattern works with any combination:

1. Expensive model: Claude Code, Cursor, Copilot — whatever you’re burning through
2. Cheap worker: DeepSeek V3, Qwen 2.5, Gemini Flash, Kimi K2.5, Llama 3.3 via Together/Groq — anything with an OpenAI-compatible API
3. Routing mechanism: CLAUDE.md for Claude Code, rules files for Cursor, system prompts for others

The ingredients are:

1. A CLI tool that calls the cheap API (30 lines of Python)
2. An instruction that tells the expensive model \*when\* to call it
3. A clear boundary: thinking stays expensive, I/O goes cheap

It took me an afternoon to build. The Python scripts are trivial. The \`CLAUDE.md\` routing rules are 20 lines. The hard part was having the idea in the first place.

If you’re burning through your AI coding limits every week, you don’t need a bigger plan. You need a worker.

**Try It Yourself**

The full implementation (all three tools + CLAUDE.md routing rules + setup script) is on GitHub: ***link to be added***

Setup takes 5 minutes:

1. Get an API key from any cheap model provider
2. Create a Python venv with the \`openai\` package
3. Drop the CLI tools in \`~/bin/\`
4. Add routing rules to your \`CLAUDE.md\`

That’s it. No plugins to install. No MCP servers to configure. No extensions to manage. Just scripts on your PATH and instructions in a markdown file.

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*J868m-kmZStk7WFORZ83Aw.png)