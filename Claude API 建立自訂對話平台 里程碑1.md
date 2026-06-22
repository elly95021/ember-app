# Build brief — Milestone 1: a working chatbot (paste into Claude Code)

Goal: one web app where a user can have a conversation with the Claude API,
deployed to a public URL, with the API key kept secure on a small backend.
This is milestone 1 only: a working chat. Entry cards, committed-action buttons,
arm assignment, data logging, and the timer come in later milestones, so do not
build them yet.

Paste this whole brief into Claude Code as your first instruction. Where it says
[PASTE PROMPT HERE], paste the Ember system prompt (Prompt 2) from
ember_prompt_pack_v11.md.

## What to build
- A single-page web chat interface. Plain HTML, CSS and JavaScript is fine; keep
  it simple, clean and readable. The user types in a text box and sends messages,
  and the bot's replies appear in a chat view.
- A small serverless backend function at `/api/chat` that:
  - reads the Anthropic API key from an environment variable `ANTHROPIC_API_KEY`
    (the key must NEVER be sent to the browser),
  - forwards the conversation to the Anthropic Messages API at
    `https://api.anthropic.com/v1/messages` (use the `x-api-key` header and the
    current `anthropic-version` header),
  - uses model `claude-sonnet-4-6`, `max_tokens: 1000`, `temperature: 0.5`,
  - sends a fixed system prompt (below) via the `system` field,
  - returns the assistant's reply text to the frontend.
- The frontend keeps the full conversation history and sends it with every
  request, so the bot remembers the conversation.
- Deploy the app to Vercel and give me the public URL.

## System prompt to use
[PASTE PROMPT HERE]

## Constraints
- The API key lives only in the backend environment variable, never in client code.
- Keep it minimal. Do not add login, a database, analytics, entry cards, buttons,
  or arm switching yet. Just a working chat.
- Make the chat readable; we will restyle it properly later.

## Things I (the researcher) am not a coder, so please guide me through:
- Getting an Anthropic API key from console.anthropic.com, and setting a small
  spend limit there.
- Adding that key as an environment variable named `ANTHROPIC_API_KEY` in Vercel.
- Creating a free Vercel account if needed and deploying, then giving me the URL.
- Explain each terminal command in one plain sentence before I run it.
