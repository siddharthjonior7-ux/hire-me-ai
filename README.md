[README.md](https://github.com/user-attachments/files/33278180/README.md)
# Hire Me AI 🎤📄

**Don't read the resume. Interview it.**

Hire Me AI turns a resume into a chatbot. Recruiters type a question — *"What are his projects?"*, *"What's his strongest tech stack?"*, *"What are his hobbies?"* — and get the answer streamed live, written straight from the resume PDF (plus an extra file for school, hobbies and other personal details).

- **Live site:** https://answer-my-cv.lovable.app
- **Backend (API):** https://hire-me-ai-cwzj.onrender.com

---

## How it works

```
Recruiter asks a question
        │
        ▼
 Frontend (Lovable)  ──POST /chat {"question": "..."}──▶  Backend (FastAPI on Render)
                                                                │
                                                reads my_resume.pdf + more_about_me.pdf
                                                (parsed once, then cached)
                                                                │
                                                asks Groq's LLM to answer using ONLY the resume
                                                                │
        ◀──────── answer streams back as plain text ────────────┘
```

- The PDFs are parsed **once** and cached, so every question after the first is fast.
- The answer streams in word-by-word (the frontend reads it with a `ReadableStream`, not EventSource).
- Nothing is stored — each question is answered fresh from the PDFs.

## Project structure

```
hire-me-ai/
├── main.py                  # FastAPI backend: parses PDFs, calls Groq, streams answers
├── requirements.txt         # Python dependencies
├── my_resume.pdf            # The resume (projects, skills, experience)
├── more_about_me.pdf        # Extra details: school, age, hobbies
├── Hire-Me-AI-Setup-Guide.md / .pdf   # Step-by-step guide to make your own copy
└── README.md
```

## Tech stack

| Part | What | Where it runs |
| --- | --- | --- |
| Backend | Python, FastAPI, Groq (`openai/gpt-oss-120b`), pypdf | Render (free tier) |
| Frontend | React + TanStack Start, Tailwind CSS | Lovable |
| LLM | Groq free API | groq.com |

## API

### `POST /chat`

Ask a question about the resume.

**Request:**
```json
{ "question": "what are siddharth's projects?" }
```

**Response:** plain text, streamed chunk-by-chunk (not JSON).

**Example:**
```bash
curl -X POST https://hire-me-ai-cwzj.onrender.com/chat \
  -H "Content-Type: application/json" \
  -d '{"question": "what are his projects?"}'
```

## Run it locally

1. Install Python 3.10+
2. Clone this repo:
   ```bash
   git clone https://github.com/siddharthjonior7-ux/hire-me-ai
   cd hire-me-ai
   ```
3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
4. Create a `.env` file with your free Groq key (get one at https://console.groq.com):
   ```
   GROQ_API_KEY=your_key_here
   ```
   ⚠️ Never commit `.env` to GitHub — the repo's `.gitignore` already blocks it.
5. Start the server:
   ```bash
   uvicorn main:app --reload
   ```

## Deploy your own (Render — free)

Full click-by-click guide: see **[Hire-Me-AI-Setup-Guide.md](Hire-Me-AI-Setup-Guide.md)**. Short version:

1. Push this repo to your own GitHub.
2. On [render.com](https://render.com) → **New → Web Service** → connect the repo.
3. Settings:
   - **Runtime:** Python 3
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `uvicorn main:app --host 0.0.0.0 --port $PORT`
4. **Environment →** add `GROQ_API_KEY` (never put it in GitHub).
5. Deploy → copy your URL.

## Make it for someone else

Each person gets their own full copy: their own GitHub repo, their own free Groq key, their own Render service, and their own site link. The [setup guide](Hire-Me-AI-Setup-Guide.md) walks through every click.

## Common problems

| Problem | Fix |
| --- | --- |
| `Internal Server Error` on first question | Check `GROQ_API_KEY` is set in Render → Environment, and both PDFs are in the repo with exact names |
| First answer takes ~30 seconds | Normal — Render's free tier sleeps after 15 quiet minutes; the first question wakes it up |
| Answers only cover projects/skills, not hobbies | `more_about_me.pdf` is missing from the repo — upload it and redeploy |
| Browser blocks the answers (CORS) | Make sure the `CORSMiddleware` block is in `main.py` |

## Credits

Built with [Lovable](https://lovable.dev), [FastAPI](https://fastapi.tiangolo.com) and [Groq](https://groq.com).
