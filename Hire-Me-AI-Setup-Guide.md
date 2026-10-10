# Hire Me AI — Full Setup Guide

Make your own version of Hire Me AI: a website where recruiters ask questions and an AI answers from **your** resume. At the end you get your own link to share.

Follow the parts in order. Do not skip steps.

---

## Before you start — what you need

1. A laptop (Windows, Mac, anything).
2. Two PDF files ready on your computer:
   - `my_resume.pdf` — your resume (projects, skills, experience).
   - `more_about_me.pdf` — extra stuff: school, age, hobbies.
   - The names must match **exactly**, including capital letters.
3. Free accounts on 4 websites (you will make them as you go):
   - [github.com](https://github.com) — stores the code
   - [console.groq.com](https://console.groq.com) — the AI brain (free)
   - [render.com](https://render.com) — runs the backend for free
   - [lovable.dev](https://lovable.dev) — makes the website page
4. Around 45–60 minutes.

---

## Part 1 — Your GitHub repo (the code + your PDFs)

1. Go to **github.com** and sign in (or click **Sign up** and make an account first).
2. Open the original project: **https://github.com/siddharthjonior7-ux/hire-me-ai**
3. Click the green **Code** button → **Download ZIP**.
4. Unzip the downloaded file on your computer. You now have a folder with all the code.
5. Go back to GitHub. Click the **+** in the top-right corner → **New repository**.
6. Repository name: `hire-me-ai`. Choose **Public**. Click **Create repository**.
7. On the new empty repo page, click the link **"uploading an existing file"**.
8. Drag in **all the files from the unzipped folder** (all code files, `requirements.txt`, etc.). Click **Commit changes**.
9. Now add your two PDFs: click **Add file → Upload files**, drag in `my_resume.pdf` and `more_about_me.pdf`, click **Commit changes**.
10. Check the file list at the top of your repo. Look for a file called `.gitignore` (starts with a dot).
    - If you see `gitignore` **without** the dot instead: click it, click the ✏️ **pencil icon**, change the file name to `.gitignore`, click **Commit changes**.
    - This file stops your secret key from ever being uploaded to GitHub.

**Your repo now has: the code + your two PDFs. This is what the AI reads to answer questions.**

---

## Part 2 — Your Groq key (the AI brain)

1. Go to **console.groq.com** and sign up / sign in.
2. In the left menu, click **API Keys**.
3. Click **Create API Key**. Give it any name, like `hire-me`.
4. **Copy the key immediately and paste it somewhere safe** (like Notepad). It is shown only once.
5. ⚠️ **Never put this key on GitHub.** It only goes into Render in Part 3, step 9.

---

## Part 3 — Render (the always-on computer)

This runs your backend so it answers questions 24/7. Free plan.

1. Go to **render.com** → click **Get Started** → **Sign up with GitHub** → authorize it.
2. On the dashboard, click **New +** → **Web Service**.
3. Find your `hire-me-ai` repo in the list → click **Connect**.
4. Fill in the settings:
   - **Name:** `hire-me-ai` (your link will be based on this name; Render may add a few random letters — that's fine)
   - **Runtime:** `Python 3`
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `uvicorn main:app --host 0.0.0.0 --port $PORT`
   - **Instance Type:** `Free`
5. Click **Create Web Service**. Watch the logs — wait until the status says **Live** (takes a few minutes).
6. Click **Environment** in the left menu → **Add Environment Variable**:
   - Key: `GROQ_API_KEY`
   - Value: paste your Groq key from Part 2
   - Click **Save**.
7. Click **Manual Deploy → Deploy latest commit**. Wait until it says **Live** again.
8. **Copy your backend URL** from the top of the Render page. It looks like:
   `https://hire-me-ai-xxxx.onrender.com`
   Keep it — you need it in Part 4.

---

## Part 4 — Lovable (the pretty page)

1. Go to **lovable.dev** and sign in.
2. Click **Create project** / **Start a project**.
3. Paste this exact message into the chat and press Enter:

```
Build a one-page site called "Hire Me AI" where recruiters type a question
and get a streaming answer from a resume. Editorial resume style: serif
display headings, clean sans-serif body, cream/paper background. It must:
- POST to {API_URL}/chat with JSON {"question": "..."}
- Read the answer as a plain-text stream with a ReadableStream reader
  (NOT EventSource), showing words appear live as they arrive
- Show 4 suggested starter questions the visitor can click
- Have a "Download resume (PDF)" button
I will give you my backend URL next.
```

4. After it builds, send another chat message with **your** backend URL from Part 3, step 8:
   `Use this backend URL: https://hire-me-ai-xxxx.onrender.com`
5. In the same project's chat, attach **your** `my_resume.pdf` and say:
   `Use this PDF for the resume download button`
6. Test it: in the preview, click one of the suggested questions.
   - ⚠️ The **first** answer can take ~30 seconds — the free Render server wakes up from sleep. Every answer after that is fast.

---

## Part 5 — Publish and share

1. When you're happy with the page, click **Publish** (top right in Lovable).
2. You get a permanent link like `your-site.lovable.app`.
3. Send that link to anyone. Done! 🎉

---

## Common problems and fixes

| Problem | Fix |
|---|---|
| Answer says `Internal Server Error` | Check step 9 of Part 3 (Groq key added in Render **Environment**, not GitHub) and that **both PDFs** are in your GitHub repo. Then **Manual Deploy → Deploy latest commit**. |
| Answer says `I don't have enough information` about school/hobbies | `more_about_me.pdf` is missing from GitHub, or was uploaded with a different name. |
| First question after a break is slow (~30s) | Normal. Render's free plan sleeps after 15 quiet minutes. It wakes up on its own. |
| Nothing shows up at all in the browser | The backend's `main.py` needs the CORS lines (allowing other sites to read its answers). Make sure you copied the latest code from the original repo. |
| Wrong answers / talks about someone else's projects | The PDFs in **your** repo are not yours, or the name doesn't match exactly. |

---

## Quick checklist

- [ ] Code uploaded to my own GitHub repo
- [ ] `my_resume.pdf` and `more_about_me.pdf` in the repo (exact names)
- [ ] `.gitignore` has the dot at the start
- [ ] Groq key created and saved somewhere safe
- [ ] Render service says **Live**
- [ ] `GROQ_API_KEY` added in Render Environment
- [ ] Frontend built in Lovable with my backend URL
- [ ] My resume PDF attached for the download button
- [ ] Tested with a question
- [ ] Published and link shared
