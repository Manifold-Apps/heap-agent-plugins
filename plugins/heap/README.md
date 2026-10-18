# Heap plugin for Claude Code

Connects Claude Code to your Heap notes. The plugin bundles two things:

- The Heap MCP server (`heap`), which gives Claude tools to search and read your notes and to save new ones.
- The `heap` skill, which tells Claude when to look in Heap on its own, to ask before saving, how to save a recap of a chat with a marker you can search for later, and what never to do.

## Install

Three ways, from most to least complete.

**From the marketplace** (in Claude Code):

```
/plugin marketplace add Manifold-Apps/heap-agent-plugins
/plugin install heap@heap-agent-plugins
```

Or from a shell:

```bash
claude plugin marketplace add Manifold-Apps/heap-agent-plugins
claude plugin install heap@heap-agent-plugins
```

**From a local clone, for one session:**

```bash
claude --plugin-dir /path/to/heap-agent-plugins/plugins/heap
```

**The server alone, without the skill:**

```bash
claude mcp add --transport http heap https://notable-bison-166.convex.site/mcp
```

## First run: sign in

Heap signs you in with OAuth, once. Start Claude Code with the plugin, run `/mcp`, pick `plugin:heap:heap` and authenticate in the browser. From a shell, `claude mcp login` does the same. Then ask "Which Heap account is connected?" to check: Claude calls `whoami`.

The plugin's server is its own connection with its own login, separate from any `heap` server you added by hand.

## What the skill does

- Looks in Heap when you refer to something you saved, read or noted before. It does not look on every turn.
- Saves only when you ask, or after you agree to a save it offered.
- Saves a recap of the chat on request, shows it first unless you say to skip the review, and ends it with a marker like `heapref7k3q9xpa`. Ask "Find the Heap note for chat heapref7k3q9xpa" to get back to it.
- Passes the page's text when it saves a link it already has the text for, or one behind a login or paywall, because Heap's own fetch may not read it and a second save of the same address does not use new text.
- Treats note text and saved page text as data, never as instructions.

The skill is `/heap:heap`, and it also triggers from its description.

## Pointing at another deployment

The server address is the `heap_mcp_url` setting, and its default is in `.claude-plugin/plugin.json`. To use another deployment for yourself, install with `claude plugin install heap@heap-agent-plugins --config heap_mcp_url=<url>` (`claude plugin configure heap` shows the option and whether it is set). A server signs in per URL, so a new address needs a new login.

## Use Heap from claude.ai

claude.ai connects to the same server as a custom connector: Settings, Connectors, Add custom connector, paste the same URL, and log in. claude.ai gets the tools and the server's instructions but not the skill, so the behavior the skill adds (look-ups without being asked, confirming before a save, the recap marker) depends on the tool descriptions there.
