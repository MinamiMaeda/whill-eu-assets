# 🚀 WHILL EU Assets — Render Deployment Guide
### Get the app online at https://whill-eu-assets.onrender.com

---

## What you need (all free)
- A **GitHub** account → github.com
- A **Render** account → render.com
- Your existing **Supabase** project

Total time: about 20 minutes.

---

## STEP 1 — Enable Supabase Storage (5 min)

You need to enable file storage in your existing Supabase project.

1. Go to https://supabase.com → open your project
2. In the left sidebar, click **Storage**
3. Click **"New bucket"**
   - Name: `asset-documents`
   - Public bucket: **OFF** (keep private — files need sign-in to access)
   - Click **Create bucket**

Now get your **Service Role Key** (needed for file uploads):

4. Go to **Settings → API**
5. Copy the **service_role** key (the long one under "Project API keys")
   ⚠️ Keep this secret — it has full access to your project

---

## STEP 2 — Put the app files on GitHub (5 min)

1. Go to https://github.com and sign in (or create a free account)
2. Click the **"+"** icon → **"New repository"**
   - Repository name: `whill-eu-assets`
   - Visibility: **Private** ← important, keeps your code private
   - Click **Create repository**

3. On the next page, click **"uploading an existing file"**
4. Drag and drop ALL files from this folder:
   - `app.py`
   - `requirements.txt`
   - `render.yaml`
   - `Procfile`
   - `.gitignore`
   - The entire `templates/` folder
   - The entire `static/` folder
   
   ⚠️ Do NOT upload `config.py` — your passwords stay off GitHub

5. Click **"Commit changes"**

---

## STEP 3 — Deploy on Render (5 min)

1. Go to https://render.com and click **"Get Started for Free"**
2. Sign up using your **GitHub account** (click "GitHub" button)
3. Click **"New +"** → **"Web Service"**
4. Click **"Connect a repository"** → select `whill-eu-assets`
5. Fill in the settings:
   - **Name:** `whill-eu-assets`
   - **Region:** Frankfurt (EU Central) — closest to you
   - **Runtime:** Python 3
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `gunicorn app:app --bind 0.0.0.0:$PORT --workers 2 --timeout 60`
   - **Instance Type:** Free
6. Click **"Create Web Service"**

Render will now build and start your app. This takes 2–3 minutes.

---

## STEP 4 — Add your secret settings on Render (3 min)

While Render is building, add your credentials:

1. On your Render service page, click **"Environment"** in the left menu
2. Click **"Add Environment Variable"** and add each of these:

| Key | Value |
|-----|-------|
| `DATABASE_URL` | `postgresql://postgres.grujmzbchocnxoyqhpxe:JohanCruijffBoulevard65!@aws-1-eu-west-1.pooler.supabase.com:6543/postgres` |
| `SUPABASE_URL` | `https://grujmzbchocnxoyqhpxe.supabase.co` |
| `SUPABASE_SERVICE_KEY` | *(the service_role key from Step 1)* |
| `SECRET_KEY` | `whill-eu-assets-2025-secret` |
| `APP_URL` | `https://whill-eu-assets.onrender.com` |

3. Click **"Save Changes"** — Render will automatically restart with the new settings

---

## STEP 5 — Open the app ✅

Once the build shows **"Live"** (green dot):

👉 **https://whill-eu-assets.onrender.com**

Share this URL with your colleague. She can open it on any device, anywhere.

---

## ⚠️ One thing to know about the free plan

On Render's free plan, the app **"sleeps"** after 15 minutes of no use.
The first person to open it each morning will wait **~50 seconds** for it to wake up.
After that, it runs normally for everyone.

To fix this permanently: upgrade to Render's "Starter" plan (~$7/month).

---

## Setting up email notifications (optional)

To send emails when approval requests are submitted, add these extra environment variables on Render:

| Key | Value |
|-----|-------|
| `EMAIL_ENABLED` | `true` |
| `EMAIL_TO` | `yuki@whill.inc,lo@whill.inc` |
| `EMAIL_FROM` | `your.gmail@gmail.com` |
| `EMAIL_USERNAME` | `your.gmail@gmail.com` |
| `EMAIL_PASSWORD` | *(Gmail App Password — see below)* |

**How to get a Gmail App Password:**
1. Go to your Google Account → Security
2. Enable 2-Step Verification (if not already)
3. Go to Security → App Passwords
4. Select "Mail" and "Windows Computer" → Generate
5. Copy the 16-character code → use as `EMAIL_PASSWORD`

---

## Making updates later

Whenever you change a file and want to update the live app:

1. Go to your GitHub repository
2. Click the file you want to change → click the ✏️ pencil icon → edit → Commit
3. Render automatically detects the change and re-deploys (takes ~2 min)

---

## Summary of all your services

| Service | What it does | URL |
|---------|-------------|-----|
| **Render** | Runs the app 24/7 | https://render.com |
| **Supabase** | Stores all data + uploaded files | https://supabase.com |
| **GitHub** | Stores the code | https://github.com |

All three are free. Your data in Supabase is independent — even if you stop Render, all data stays safe.
