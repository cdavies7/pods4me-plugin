---
name: queue-podcast-episodes
description: Find podcast episodes and queue them into the user's private pods4.me feed. Use when the user wants to listen to a podcast episode later, save or add an episode to their feed or queue, find episodes on a topic or with a guest, or set up their pods4.me feed in a podcast player.
---

# Queue podcast episodes with pods4.me

pods4.me gives the user a private podcast feed that an agent fills for them. Anything queued here shows up in their own podcast player. These instructions use the `pods4me` MCP server bundled with this plugin.

## When to use

- "Save this episode for later" or "add this to my podcast feed", with a link, a title, or a description.
- "Find me an episode about X" or "the episode where Y talked about Z".
- "What's in my queue?" or "remove that episode".
- "How do I get this feed into my podcast app?"

## First check the connection

If the pods4me tools are missing or return an authentication error, ask the user to enable the pods4me MCP server in Cursor and sign in. Sign-in uses OAuth with an email magic link, and an account and private feed are created on first sign-in. Never ask the user to paste a token into chat.

To check the connection without changing anything, call `list_queue` or `list_podcast_sources`.

## Find the episode

Choose the tool that fits what the user gave you:

| The user gave you | Tool |
| --- | --- |
| An episode link (Apple Podcasts, Spotify, publisher page, audio file) | `add_episode_to_queue` with `input` set to the URL, or `find_podcast_episodes` first to show what it matched |
| A show name, episode title, or guest | `find_podcast_episodes` with `query` |
| A topic across all shows | `find_episodes_by_topic` with `topic` |
| "Something like what I usually like" | `recommend_episodes`, with an optional `topic` |
| "That episode I heard before" | `search_listening_history` |

## Show the evidence and let the user choose

Before adding anything, show what you found. For each candidate give:

- the podcast name and episode title,
- the publication date,
- the duration if known,
- one line on why it matches what the user asked for (guest, topic, or title).

When exactly one episode clearly matches, show it and ask "Add this to your feed?"

When more than one could be the match, for example several episodes with the same guest, a vague description, or a show with similar titles, show a short numbered list (up to 5) and ask which one. Don't guess. If nothing matches well, say so and suggest a better search term rather than queueing a near miss.

## Queue it

Every change needs the user's explicit confirmation first.

- Call `add_episode_to_queue` with the stable `episode_id` from the search result the user picked. Pass only one of `episode_id` or `input`.
- If `add_episode_to_queue` returns choices, show them with their dates, ask which one, and call it again with that choice.
- After it succeeds, confirm the title and podcast you added.

Other changes (all need confirmation):

- Remove an episode: call `list_queue`, confirm which one, then `remove_episode_from_queue`.
- Follow a show: `search_podcast_sources`, then `add_podcast_source` with `selection_mode` set to `best` (episodes picked by the user's preferences) or `all` (every new episode).
- Preferences, weekly limit, or feed name: `get_preferences`, then `update_preferences`.

## Add the private feed to a podcast player

The agent never sees the private RSS address, and it must stay out of chat.

1. Call `get_subscription_link`. It returns a single-use page that is valid for 45 minutes.
2. Give that link to the user and ask them to open it on the device where they listen. Don't open it yourself, and don't post it anywhere else.
3. On that page the user picks their player. Supported players include Pocket Casts, Apple Podcasts, Overcast, Castro, AntennaPod, and other apps that accept a private RSS URL.
4. Spotify can't be used, because it doesn't accept private RSS feeds. If the user mainly listens on Spotify, say so plainly and suggest one of the other players.

Only give app-specific steps if you know which player the user has.

## Transcripts

`get_episode_transcript` reads transcripts of episodes the user has queued, in chunks. To keep reading, pass the returned `next_offset` back as `offset`. Use transcripts to answer questions or write summaries for the user, and don't republish them.
