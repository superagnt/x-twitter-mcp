---
name: x-account-reader
description: "This skill should be used when the user wants their agent to read a specific person's X (Twitter) posts, or asks \"what has @<handle> posted\", \"read @<handle>'s tweets\", \"what did <person> say on X about <topic>\", \"summarize this account's posts this month\", \"who replied to this tweet\", or \"search X for posts from <handle>\". Runs on the superagnt MCP server's X tools: an account's profile and posts, X search with its own operators, and a post's replies. Read only: it never posts, likes, follows or sends a message."
version: 0.1.0
---

# X account reader

Read what one X account actually posted, not what the web says about the
person: their posts in a window, the replies they drew, and what they keep
coming back to, each point linked to its post.

It only reads. It never posts, likes, reposts, follows or sends a message.
It reads public accounts: a protected account's posts are visible only to the
followers it approved, so they can't be read here.

**Tools:** `data_x_users`, `data_x_discover` and `data_x_posts`, from the
superagnt MCP server (`https://mcp.superagnt.com/mcp`). If none of them is
listed, the server isn't connected yet: the setup guide is
https://www.superagnt.com/install.

## Step 1: the account and the window

Ask once, in one message, for what's missing:

- the handle (with or without the @);
- the window: the last week, two weeks, month, or dates;
- a topic to narrow to, if any.

## Step 2: who it is

`data_x_users` with `action` set to `details` and `username` set to the handle.
Check the account exists and is the person the user means (name, bio,
follower count). If it's protected, say its posts can't be read, and stop.

## Step 3: their posts

Two ways in, by what the user asked:

- **Everything in a window:** `data_x_discover` with `action` set to `search`,
  `query` set to `from:<handle>`, `section` set to `latest`, and `start_date`
  (and `end_date`) as `YYYY-MM-DD`. Add words to the query to narrow it to a
  topic (`from:<handle> pricing`), and `min_likes` to keep only the posts that
  landed.
- **Their timeline as it stands:** `data_x_users` with `action` set to
  `tweets`, `username` set to the handle and `limit` set to how many. Set
  `include_replies` to `true` to see their replies to others too, and
  `include_pinned` to `false` to leave the pinned post out.

Either result carries a `continuation_token`: pass it back with the same
action to read the next page, until the window is covered.

## Step 4: the conversation around a post

For the posts that matter (the most liked, or the ones on the topic),
`data_x_posts` with `action` set to `replies` and `tweet_id` set to the post's
id reads what people said back. `details` reads one post in full.

## Step 5: the answer

- What the person said, in their own words, quoted short, each with its date
  and link;
- the themes they return to, with how many posts each came up in;
- the posts that drew the most likes or replies, and what the replies said.

Say how many posts the answer read and the window it covered. Never invent a
post, a quote or a number; when a call fails or returns nothing, say so.
