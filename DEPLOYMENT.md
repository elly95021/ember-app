# Deployment Guide — Ember Chat App

This app now supports two study arms and logs chat sessions to Supabase.

## Step 1: Get Your Anthropic (Claude) API Key

This is where you get permission to use Claude's AI.

1. Go to **console.anthropic.com** in your browser
2. Sign in (or create a free account if you don't have one)
3. On the left side, find **"API Keys"** and click it
4. Click **"Create Key"**
5. Give it a name like "Ember app" (this is just for you to remember what it's for)
6. Copy the key (it starts with `sk-ant-`)
7. **Save it somewhere safe** — you'll need it in a moment. Don't share it with anyone.

### Optional: Set a Spending Limit (Recommended)

To avoid surprise costs:
1. Go to **Plans** in the left menu
2. Find **"Monthly Budget"**
3. Set it to something small like $5 (you can change it anytime)

## Step 2: Create Supabase for Logging

The app saves chat transcripts to Supabase so you can export them later.

1. Go to **supabase.com** in your browser
2. Click **"Start your project"** and sign up or log in
3. Create a new project with any name you like
4. In project settings, go to **API** and copy the **Project URL**
5. In project settings, go to **Database** > **Settings** > **Database Connection**
   and copy the **Service Role** key
6. Keep both values secret. The service key must never be sent to the browser.

### Create the logging table in Supabase

Use Supabase SQL editor and run:

```sql
create table if not exists session_logs (
  id text primary key,
  group text not null,
  pid text,
  start_time timestamp with time zone,
  end_time timestamp with time zone,
  messages jsonb
);
```

This table stores:
- `id`: a session identifier
- `group`: `ember` or `info`
- `pid`: participant ID from the URL
- `start_time` and `end_time`
- `messages`: all chat messages in order

## Step 3: Deploy to Vercel

1. Go to vercel.com and sign in with GitHub
2. Add your GitHub repo if you haven't yet
3. Import the project and deploy it
4. The app files should include:
   - `index.html`
   - `api/chat.js`
   - `vercel.json`
   - `package.json`

## Step 4: Add Environment Variables in Vercel

In your Vercel project settings, add these variables exactly:

- `ANTHROPIC_API_KEY` = your Anthropic key
- `SUPABASE_URL` = your Supabase project URL
- `SUPABASE_SERVICE_KEY` = your Supabase service role key

After adding them, redeploy the project from the Vercel dashboard.

## Step 5: Test the two arms

Use these test URLs once the app is deployed:
- `https://<your-app>.vercel.app/?group=ember&pid=1234`
- `https://<your-app>.vercel.app/?group=info&pid=1234`

If `group` is missing or invalid, the app shows a short neutral message and does not start chat.

## Step 6: Confirm logging

To check logged data:
1. Open your Supabase project
2. Go to **Table Editor**
3. Open `session_logs`

You should see rows for each session with `group`, `pid`, `start_time`, `end_time`, and `messages`.

If you want to download the data later, use the Supabase table export or SQL editor export tools.
