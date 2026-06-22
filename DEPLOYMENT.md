# Deployment Guide — Ember Chat App

You're going to get an API key, then deploy this app to Vercel (a free hosting platform). I'll walk you through each step.

---

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

---

## Step 2: Create a Vercel Account (One-Time)

Vercel is where your app will live on the internet.

1. Go to **vercel.com** in your browser
2. Click **"Sign Up"** in the top right
3. Choose "Continue with GitHub" (easiest option):
   - If you don't have GitHub, create a free account at github.com first, then come back here
   - GitHub will ask permission; click **"Authorize"**
4. Vercel will ask for your name — enter it and complete signup
5. You should now see a dashboard with a big **"Create Project"** button

---

## Step 3: Deploy This App to Vercel

Now you'll upload your chat app.

### Option A: Using Vercel's Web Interface (Easiest)

1. Make sure your folder has these files:
   - `index.html`
   - `api/chat.js`
   - `vercel.json`
   - `package.json`

2. Go to your Vercel dashboard
3. Click **"Add New..."** → **"Project"**
4. Click **"Import Git Repository"**
5. Paste this in the URL field:
   ```
   https://github.com/username/ember-app
   ```
   (Or, if you uploaded it to GitHub, use your GitHub repo URL)

   **If you don't have it on GitHub yet:**
   - Go to github.com
   - Click **"New repository"**
   - Name it `ember-app`
   - Upload your files there first, then come back to Vercel

6. Once Vercel shows your repo, click **"Import"**
7. Vercel will show configuration options — just click **"Deploy"** (the defaults are fine)
8. Wait 1-2 minutes for deployment to finish

### Option B: Using Terminal (If You're Comfortable)

1. Open Terminal (Mac) or Command Prompt (Windows)
2. Navigate to your ember-app folder:
   ```
   cd /Users/ellielin/Desktop/ember-app
   ```
3. Install Vercel's command line tool:
   ```
   npm install -g vercel
   ```
4. Deploy:
   ```
   vercel
   ```
5. Follow the prompts and select "Y" for the defaults

---

## Step 4: Add Your API Key to Vercel (The Important Bit)

Once deployment is done, you'll have a URL like `https://ember-app-xyz.vercel.app`.

Now tell Vercel where to find your API key:

1. Go to vercel.com and find your **Ember** project
2. Click on the project name
3. Go to the **Settings** tab (top of the page)
4. On the left, click **"Environment Variables"**
5. Click **"Add New"**
6. In the **"Name"** field, type exactly:
   ```
   ANTHROPIC_API_KEY
   ```
7. In the **"Value"** field, paste your API key (the `sk-ant-...` you saved earlier)
8. Click **"Add"**

Now redeploy so Vercel picks up the new environment variable:
- Go back to the **Deployments** tab
- Find the most recent deployment
- Click the three dots ⋯
- Click **"Redeploy"**
- Wait 1-2 minutes

---

## Step 5: Test Your App

Once redeployed:

1. Go to your Vercel URL: `https://ember-app-xyz.vercel.app`
2. Type a message in the chat box
3. Click **Send** or press Enter
4. If Ember replies, you're done! 🎉

If it doesn't work:
- Make sure the API key is added (Step 4)
- Make sure it's redeployed after adding the key
- Check your API spending limit at console.anthropic.com hasn't been exceeded

---

## Your Final URL

Once deployed, share this URL with your research team or users:

```
https://ember-app-[your-unique-code].vercel.app
```

(Vercel will give you the exact URL when deployment finishes.)

---

## What Each File Does

- **index.html** — The chat screen users see
- **api/chat.js** — The invisible backend that talks to Claude; **your API key lives here** (in the environment variable, not in the code itself)
- **vercel.json** — Configuration so Vercel knows how to run this app
- **package.json** — Metadata about the project

---

## Questions?

- **"Where does my API key live?"** → In Vercel's environment variables, nowhere near the public code
- **"Is it free?"** → Yes, Vercel is free. Claude API charges small amounts (you set a limit to be safe)
- **"Can I change the system prompt?"** → Yes, edit `api/chat.js` line by line where it says `const systemPrompt = ...`
- **"How many people can use it?"** → As many as you want (within your API spending limit)

