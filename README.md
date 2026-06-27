# Ember — Screening Support Chat

A web application for providing evidence-based screening support using Acceptance and Commitment Therapy (ACT).

This version supports two study arms:
- `?group=ember` uses the Ember ACT prompt
- `?group=info` uses the neutral information prompt

It also logs chat sessions to Supabase with participant ID, group, full conversation, and timestamps.

## Files

- `index.html` - The chat interface that users see
- `api/chat.js` - The backend that safely handles API calls and logging
- `DEPLOYMENT.md` - Deployment and Supabase setup instructions

## Notes

The `pid` parameter is accepted from the URL for participant tracking.
The group assignment is controlled by the `group` URL parameter only.
The interface looks the same for both groups.
