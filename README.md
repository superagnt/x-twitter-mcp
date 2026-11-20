# x-twitter-mcp

To get Claude to read a specific person's X posts, connect this X (Twitter) MCP server: Claude, ChatGPT or Cursor then reads any public account's posts and searches X.

This repo holds the client config for superagnt's hosted MCP server and a skill that uses its X tools. The server is hosted: point your client at the URL below and sign in. There's no X API key to get and nothing to run on your machine. Claude's own web search can surface a post a search engine indexed; it can't read an account's timeline.

## Install

**Server URL:** `https://mcp.superagnt.com/mcp` (Streamable HTTP, OAuth). Your client opens superagnt's consent screen in your browser, and approving it creates your superagnt account if you don't have one. There's no API key to copy. The setup guide for every client, with a line you can paste into your agent: https://www.superagnt.com/install

**Claude** (claude.ai, the desktop app and Cowork): Customize, then Connectors, then Add custom connector. Name it superagnt, paste the server URL, add it and approve. One connector covers Claude on the web, the desktop app, mobile, Cowork and Claude Code signed in with your Claude account. Free Claude plans can add one custom connector.

**ChatGPT:** In ChatGPT on the web, go to Plugins, select the plus button, then Add custom MCP server. Name it superagnt, enter the server URL under Connection, and set Authentication to OAuth. Tick "I understand and want to continue", then select Create as a plugin. Install the plugin, then start a new chat, type @ and select superagnt. ChatGPT takes custom MCP servers on the web only, and OpenAI lists them for Business, Enterprise/Edu and Pro plans. Pro accounts get read and fetch actions only, which covers every X tool here.

**Cursor:** put [`config/cursor.mcp.json`](config/cursor.mcp.json) in your project as `.cursor/mcp.json`, then click Login for superagnt under Settings, then MCP.

**Claude Code:**

```bash
claude mcp add --scope user --transport http superagnt https://mcp.superagnt.com/mcp
claude mcp login superagnt
```

**Codex:**

```bash
codex mcp add superagnt --url https://mcp.superagnt.com/mcp
codex mcp login superagnt
```

**Other clients**, each file in its client's own shape:

| Client | Config | Where it goes |
|---|---|---|
| Claude Code, one project | [`config/claude-code.mcp.json`](config/claude-code.mcp.json) | `.mcp.json` in the project |
| VS Code | [`config/vscode.mcp.json`](config/vscode.mcp.json) | `.vscode/mcp.json` (the key is `servers`, not `mcpServers`) |
| Gemini CLI | [`config/gemini-cli.settings.json`](config/gemini-cli.settings.json) | `~/.gemini/settings.json` (the key is `httpUrl`), then `/mcp auth superagnt` |
| Windsurf | [`config/windsurf.mcp_config.json`](config/windsurf.mcp_config.json) | `~/.codeium/windsurf/mcp_config.json` |
| Zed | [`config/zed.settings.json`](config/zed.settings.json) | `~/.config/zed/settings.json` |
| Codex, by file | [`config/codex.config.toml`](config/codex.config.toml) | `~/.codex/config.toml` |

**The skill:** `npx skills add superagnt/x-twitter-mcp` installs [x-account-reader](skills/x-account-reader/SKILL.md) into any agent that reads SKILL.md files, such as Claude Code, Codex and Cursor. Give it a handle and a window, and it reads that account's posts and the replies to them, then sums up what the person said.

A new superagnt workspace starts with the X tools on.

## What each tool does

| Tool | Actions | What it does |
|---|---|---|
| `data_x_users` | `details`, `about`, `tweets`, `media`, `likes`, `followers`, `following` | Reads an account: its profile, its posts and media, its likes, and who it follows and who follows it. `tweets` takes a `username`, with `include_replies` and `include_pinned`. |
| `data_x_discover` | `search`, `hashtag`, `geo_search`, `list_details`, `list_tweets`, `trends`, `trend_locations` | Searches posts with X's own operators (`from:`, dates, minimum likes, language), top or latest. Also reads a hashtag, a list's details and posts, and trending topics by location. |
| `data_x_posts` | `details`, `replies`, `favoriters` | Reads one post, its replies and the accounts that liked it. |
| `data_x_text` | `topics`, `detect_language` | Names the topics of a text and detects its language. |

Every tool only reads. Nothing here posts, likes, follows or sends a message. Each call uses credits from your superagnt workspace: https://www.superagnt.com/pricing

## Prompts to try

- "What has @<handle> posted on X in the last two weeks? Newest first, with the replies that got the most likes."
- "Read @<handle>'s posts this month and tell me what they keep coming back to."
- "Search X for posts from:<handle> about <topic> since 2026-01-01, and quote each with its date and link."
- "Who replied to this post, and what did they say? <post link>"
- "Find posts with at least 100 likes about <topic> this week, latest first."

A timed run of the first kind of prompt, with every call shown: https://www.superagnt.com/claude/twitter

## About superagnt

superagnt is a marketing plugin for Claude and ChatGPT that helps businesses find customers through outbound, SEO and ads. X is one of its data sources, beside Reddit, LinkedIn, TikTok, YouTube, the Meta Ad Library, Google rankings and keywords, and a database that keeps what each run finds. More: https://www.superagnt.com

## License

This repo's config and skill are MIT licensed. See [LICENSE](LICENSE). The server is superagnt's hosted service.
