# pods4.me plugin for Cursor

<img src="assets/logo.png" alt="pods4.me logo" width="96" />

[pods4.me](https://pods4.me) is a podcast feed run by your AI agent. Ask Cursor to save an episode for later, and it finds the episode and adds it to your own private RSS feed. You then listen in the podcast player you already use.

This plugin adds two things to Cursor:

- **MCP server** `pods4me` at `https://pods4.me/mcp`. It's hosted, signs you in with OAuth, and needs no API keys.
- **Skill** `queue-podcast-episodes`. It tells the agent how to find the right episode, show you the evidence, let you choose when the match is ambiguous, and help you add your private feed to your player.

## Install

1. Install **pods4me** from the Cursor Marketplace.
2. Enable the `pods4me` MCP server when Cursor asks.

## Sign in

The first time the agent uses pods4.me, Cursor opens a sign-in window:

1. Enter your email address. pods4.me sends you a magic link.
2. Open the link in the same browser. New accounts get a private feed automatically.
3. Approve the connection.

Sign-in uses OAuth 2.1 with PKCE. You never copy or paste a token, and the Cursor connection gets its own credential, so it can't affect connections you've set up in other agents.

## Usage examples

- "Add the latest Lex Fridman episode to my podcast feed."
- "Save this for later: https://podcasts.apple.com/…"
- "Find me a good episode about the history of the transistor."
- "What's in my pods4.me queue?"
- "Remove the second episode from my queue."
- "Follow Acquired, but only add the best episodes."
- "How do I listen to my feed in Overcast?"

The agent always asks before changing your queue, sources, or preferences. If several episodes could match, it shows them with podcast, date, and the reason each one matched, and you choose.

## Supported podcast players

Any player that accepts a private RSS feed URL works, including:

- Pocket Casts
- Apple Podcasts
- Overcast
- Castro
- AntennaPod

**Spotify isn't supported**, because it doesn't accept private RSS feeds.

To add your feed, ask the agent for a subscription link. You get a single-use page that's valid for 45 minutes. Open it on the device you listen on and pick your player.

## Privacy

- The agent never sees your private RSS address. It's shown only on the subscription page, which you open yourself.
- Every request is tied to your signed-in account and is rate limited.
- This repository has no code that runs on your machine. It contains only the plugin manifest, the MCP server URL, and the skill instructions.
- See [pods4.me](https://pods4.me) for the full privacy policy and terms.

## Support

Email [help@pods4.me](mailto:help@pods4.me). For other AI clients, such as Claude, ChatGPT, and Codex, see the docs at <https://pods4.me/docs/agent>.

## License

[MIT](LICENSE) © 2026 Chris Davies
