Milestone 2 — What I changed and what you need to do

Summary (simple)
- I updated the frontend so the interface is neutral and identical for both study arms.
- The backend already contained both system prompts and will send the correct one based on the `group` value passed from the browser (`ember` or `info`).
- I added this instructions file explaining how to set up data logging using Supabase and how to test the two arms.

Files changed
- index.html — replaced the visible app title so the UI does not reveal which arm the participant is in.

How the app reads assignment and pid
- The app reads `group` and `pid` from the URL query string (for example `?group=ember&pid=1234`).
- `group` must be exactly `ember` or `info`. If missing or invalid, the app will show a short neutral message and not start the chat.
- `pid` (participant id) is optional — if missing it will be recorded as an empty string.

What I used for logging (recommended option)
- I recommend using Supabase (a hosted Postgres with a simple REST API) to store session logs.
- The backend is already written to POST session records into a Supabase table named `session_logs`.

Supabase setup (steps for you — simple)
1. Create a free Supabase account at https://supabase.com and sign in.
2. Create a new project and choose a password.
3. In the Supabase dashboard, open the SQL editor and run the table creation SQL below.

-- SQL to create the table
CREATE TABLE IF NOT EXISTS session_logs (
  id text PRIMARY KEY,
  group text,
  pid text,
  start_time timestamptz,
  end_time timestamptz,
  messages jsonb
);

4. In the Project Settings > API, copy the `Project URL` (this is `SUPABASE_URL`).
5. In Project Settings > Service Key, copy the `service_role` key (this is `SUPABASE_SERVICE_KEY`).
   - Keep this key secret.

Environment variables (Vercel)
- In your Vercel project dashboard, go to Settings -> Environment Variables and add the following keys (do not paste them in client-side code):
  - `ANTHROPIC_API_KEY` — your Anthropic/Claude API key (already used by the backend)
  - `SUPABASE_URL` — the Project URL from Supabase
  - `SUPABASE_SERVICE_KEY` — the `service_role` key from Supabase

Notes about security
- The Supabase service key must stay only in Vercel environment variables (or other server environment). Never put it in the browser.
- Do not log into Vercel from the terminal — deploy via GitHub / Vercel dashboard as you already prefer.

Deploying
- Push these changes to GitHub (I will commit and push them). Then open Vercel dashboard and trigger a redeploy from the updated GitHub repo.

Testing the two arms (URLs)
- Once deployed, test the two arms by opening these URLs (replace `https://your-deployment` with your real site URL):
  - Ember arm: https://your-deployment/?group=ember&pid=TEST123
  - Information arm: https://your-deployment/?group=info&pid=TEST123
- Use a distinct `pid` so you can find the session in Supabase.

How to confirm the logged data (simple)
- In the Supabase dashboard, go to Table Editor and open `session_logs` to view rows.
- Each row contains:
  - `id`: session id
  - `group`: `ember` or `info`
  - `pid`: the pid from the URL (may be empty)
  - `start_time` and `end_time`
  - `messages`: a JSON array of role/content/timestamp entries in order
- You can export the table to CSV from the Table Editor or run SQL like:
  SELECT * FROM session_logs ORDER BY start_time DESC LIMIT 50;

If you want a different data store
- Alternatives include appending rows to a Google Sheet or using another hosted DB. I chose Supabase because it is simple to set up and gives you direct export options.

If something goes wrong
- If chats don't appear in the table, check these things in order:
  1. Are `SUPABASE_URL` and `SUPABASE_SERVICE_KEY` set in Vercel and the site redeployed?
  2. Is the Supabase table `session_logs` created with the correct schema?
  3. Check the Vercel server function logs for errors (they usually show helpful messages).

Questions I will ask you if you want me to finish setup
- Do you want me to add server-side code to write to Google Sheets instead of Supabase?
- If you want, I can add a tiny admin API endpoint to download recent sessions as CSV (requires storing a read-only key).


Thank you — I kept the UI neutral so participants cannot see their arm. If you want, I can also:
- Add a first-server-generated assistant message instead of the client-side greeting (keeps everything driven by the backend), or
- Add an export endpoint to download logs directly from the API.

