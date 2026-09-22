# Hire Me AI

A conversational assistant that lets a recruiter ask questions about me and get answers generated strictly from my actual resume — no guessing, no filler. This is a personal site: it always answers as one candidate, using one resume.

## How it works

1. The candidate's resume (PDF) is parsed once into structured JSON — name, skills, experience, education, projects, certifications — using the Groq API with a Pydantic schema.
2. A recruiter sends a question to `POST /chat`.
3. The LLM answers using only the parsed resume content, and the answer is streamed back token-by-token.

## Tech stack

- FastAPI
- Groq API (`openai/gpt-oss-120b`)
- Pydantic
- pypdf

## Project status

- ✅ **Phase 1 — Backend:** resume parsing + streaming Q&A API
- 🚧 **Phase 2 — Frontend + deployment:** in progress

## Setup

```bash
git clone <this-repo-url>
cd hire-me-ai
python -m venv venv
source venv/bin/activate   # venv\Scripts\activate on Windows
pip install -r requirements.txt
```

Create a `.env` file in the project root:

```
GROQ_API_KEY=your_key_here
```

Place a resume PDF in the project root named `my_resume.pdf`.

Run the server:

```bash
python main.py
```

The API will be available at `http://127.0.0.1:8000`.

## API reference

### `GET /`
Health check.

### `POST /chat`

Request body:

```json
{ "question": "What experience does the candidate have with cloud platforms?" }
```

Response: a streamed, plain-text answer.

## Roadmap

- [ ] Frontend (React / Next.js)
- [ ] Deploy to a public URL
