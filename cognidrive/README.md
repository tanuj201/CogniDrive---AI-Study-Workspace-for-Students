<div align="center">

# CogniDrive

**AI-powered document workspace for students — like NotebookLM meets Google Drive**

Upload lecture PDFs. Chat with multiple AI models. Generate podcast summaries, mind maps, and study tables — all in one place.

<br />

[![Live Demo](https://img.shields.io/badge/Live_Demo-Visit_App-7c3aed?style=for-the-badge&logo=vercel&logoColor=white)](https://hackathon-chi-topaz.vercel.app)
[![Next.js](https://img.shields.io/badge/Next.js-15-black?style=flat-square&logo=next.js)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19-61dafb?style=flat-square&logo=react&logoColor=white)](https://react.dev/)
[![Supabase](https://img.shields.io/badge/Supabase-PostgreSQL-3ecf8e?style=flat-square&logo=supabase&logoColor=white)](https://supabase.com/)
[![OpenRouter](https://img.shields.io/badge/AI-OpenRouter-6366f1?style=flat-square)](https://openrouter.ai/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.7-3178c6?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

[Live Demo](https://hackathon-chi-topaz.vercel.app) · [Report Bug](https://github.com/tanuj201/Hackathon/issues) · [Request Feature](https://github.com/tanuj201/Hackathon/issues)

</div>

---

## Overview

CogniDrive is a full-stack web app built for **students and researchers** who want NotebookLM-style intelligence without juggling multiple tools. Store documents in the cloud, ask questions with retrieval-augmented chat, and use Studio Tools to turn dense PDFs into audio summaries, visual mind maps, and exportable data tables.

> **Launch month:** All users get Pro-tier limits free during early access — no credit card required. Stripe billing can be enabled later via environment flags.

---

## Features

### Documents
- Upload **PDF, TXT, and CSV** with drag-and-drop
- Per-user cloud storage with quota tracking
- In-app document viewer and file management
- Secure delete with confirmation

### Multi-AI Chat
Switch models on the fly via [OpenRouter](https://openrouter.ai/):

| Model | Provider |
|-------|----------|
| Gemini 2.5 Flash | Google |
| DeepSeek V3 | DeepSeek |
| Llama 3.3 70B | Meta (free tier on OpenRouter) |
| GPT-4o | OpenAI |

### RAG (Retrieval-Augmented Generation)
- Documents split into **500-word chunks**
- Embeddings stored in **pgvector**
- Relevant context injected into every chat reply

### Studio Tools
NotebookLM-inspired tools for active studying:

| Tool | What it does |
|------|----------------|
| **Audio Overview** | Two-host podcast script + browser TTS (optional MP3 via OpenAI/ElevenLabs) |
| **Mind Map** | Interactive React Flow graph with zoom, pan, and collapsible nodes |
| **Data Table** | Structured extraction with search, filter, and CSV export |

### Auth & Accounts
- Email/password sign-up and sign-in
- **Continue with Google** (Supabase OAuth)
- Row-level security — each user only sees their own files

### Launch & Billing
- **Early access mode** — Pro limits for everyone during launch (`FREEMIUM_ENABLED=false`)
- **Student Pro** freemium ready — Stripe checkout, webhooks, and usage metering ($1.99/mo)
- Monthly limits for chat, studio runs, and storage

---

## Screenshots

<!-- Add screenshots to /docs and uncomment the lines below:

![CogniDrive Workspace](docs/workspace.png)
![Studio Tools](docs/studio.png)

To add: save PNGs to a `docs/` folder and reference them here. -->

_Screenshots coming soon — clone the repo and run locally to explore the workspace._

---

## How It Works

```
1. Upload  →  Drop a PDF or notes file into the sidebar
2. Select  →  Pick a document and choose Chat or a Studio tool
3. Learn   →  Get AI answers, mind maps, podcasts, or tables instantly
```

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Framework | Next.js 15 (App Router), React 19 |
| Styling | Tailwind CSS, Shadcn UI |
| Visualization | @xyflow/react (React Flow) |
| Database | Supabase PostgreSQL + pgvector |
| Auth | Supabase Auth, `@supabase/ssr`, middleware session refresh |
| Storage | Supabase Storage (`cognidrive-files` bucket) |
| AI Gateway | OpenRouter (chat, embeddings, studio JSON) |
| TTS | OpenAI TTS or ElevenLabs (optional) |
| Payments | Stripe (optional during launch) |
| PWA | Web app manifest + installable icon |
| Hosting | Vercel |

---

## Quick Start

### Prerequisites
- Node.js 18+
- A [Supabase](https://supabase.com) project
- An [OpenRouter](https://openrouter.ai/keys) API key

### 1. Clone and install

```bash
git clone https://github.com/tanuj201/Hackathon.git
cd Hackathon/cognidrive   # adjust path if your folder name differs
npm install
```

### 2. Environment variables

```bash
cp .env.example .env.local
```

Fill in at minimum: `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY`, `OPENROUTER_API_KEY`, and `NEXT_PUBLIC_SITE_URL`.

### 3. Supabase setup

1. Run [`supabase/schema.sql`](supabase/schema.sql) in the SQL Editor
2. Run [`supabase/schema-v2-auth-billing.sql`](supabase/schema-v2-auth-billing.sql)
3. Create a **private** storage bucket named `cognidrive-files`
4. **Authentication → Providers** — enable Email and Google (optional)
5. **Authentication → URL Configuration** — add redirect URLs:
   ```
   http://localhost:3000/auth/callback
   http://localhost:3000/auth/confirm
   ```

### 4. Run locally

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) — landing page at `/`, workspace at `/app` after login.

---

## Deploy to Vercel

### 1. Connect the repository
- Import the GitHub repo in [Vercel](https://vercel.com)
- Set **Production Branch** to `master` (active development branch)
- Root directory: `cognidrive` if the app lives in a subfolder

### 2. Add environment variables

Apply to **Production**, **Preview**, and **Development**:

| Variable | Required | Description |
|----------|----------|-------------|
| `NEXT_PUBLIC_SUPABASE_URL` | Yes | Supabase project URL |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Yes | Supabase anon public key |
| `SUPABASE_SERVICE_ROLE_KEY` | Yes | Service role key (uploads + server ops) |
| `OPENROUTER_API_KEY` | Yes | OpenRouter API key |
| `NEXT_PUBLIC_SITE_URL` | Yes | Your live URL, e.g. `https://hackathon-chi-topaz.vercel.app` |
| `NEXT_PUBLIC_SITE_NAME` | Yes | `CogniDrive` |
| `FREEMIUM_ENABLED` | Yes | `false` during launch month |
| `LAUNCH_FREE_UNTIL` | Yes | Launch end date, e.g. `2026-09-03` |
| `OPENAI_API_KEY` | No | MP3 audio overview (server TTS) |
| `ELEVENLABS_API_KEY` | No | Alternative TTS provider |
| `STRIPE_*` | No | Required only when `FREEMIUM_ENABLED=true` |

### 3. Supabase production URLs

Update **Site URL** and **Redirect URLs** to match your Vercel domain:

```
https://YOUR-APP.vercel.app/auth/callback
https://YOUR-APP.vercel.app/auth/confirm
```

### 4. Redeploy and verify

After saving env vars: **Deployments → Redeploy** (env changes do not apply until redeploy).

Health check:

```
https://YOUR-APP.vercel.app/api/status
```

Expect `ready: true`. A yellow setup banner appears in the app when something is misconfigured.

---

## Project Structure

```
src/
├── app/
│   ├── page.tsx                    # Landing page
│   ├── login/page.tsx              # Auth (email + Google)
│   ├── app/page.tsx                # Main workspace
│   ├── auth/
│   │   ├── callback/route.ts       # OAuth PKCE callback
│   │   └── confirm/page.tsx        # Hash-token fallback
│   ├── privacy/  terms/            # Legal pages
│   └── api/
│       ├── chat/                   # RAG multi-model chat
│       ├── files/                  # Upload, list, delete
│       ├── account/                # Plan + usage
│       ├── status/                 # Health check
│       ├── stripe/                 # Checkout, portal, webhook
│       └── studio/
│           ├── audio/              # Podcast + TTS
│           ├── mindmap/            # Mind map JSON
│           └── table/              # Data extraction
├── components/
│   ├── chat/                       # Chat panel + model switcher
│   ├── storage/                    # Sidebar, uploader, viewer
│   ├── studio/                     # Audio, mind map, table UI
│   ├── account/                    # Upgrade panel, sign out
│   ├── landing/                    # Marketing page
│   └── ui/                         # Shadcn primitives
├── lib/
│   ├── supabase/                   # Client + server auth helpers
│   ├── openrouter.ts               # LLM calls + model fallbacks
│   ├── rag.ts                      # Chunking + vector search
│   ├── document-parser.ts          # PDF/TXT/CSV parsing
│   ├── tts.ts                      # Text-to-speech
│   ├── usage.ts / plans.ts         # Metering + plan limits
│   └── launch-config.ts            # Early access / freemium flags
├── middleware.ts                   # Auth guard for /app
└── types/index.ts                  # Shared TypeScript types

supabase/
├── schema.sql                      # Core tables + pgvector
└── schema-v2-auth-billing.sql      # Profiles, usage, RLS
```

---

## API Routes

| Method | Route | Description |
|--------|-------|-------------|
| `GET` | `/api/status` | Service health check |
| `GET` | `/api/account` | User plan, usage, launch info |
| `GET` | `/api/files` | List files + storage quota |
| `POST` | `/api/files` | Upload, parse, chunk, embed |
| `DELETE` | `/api/files/[id]` | Delete file + chunks |
| `POST` | `/api/chat` | RAG chat with model selection |
| `POST` | `/api/studio/audio` | Podcast transcript + optional MP3 |
| `POST` | `/api/studio/mindmap` | Hierarchical mind map JSON |
| `POST` | `/api/studio/table` | Structured table extraction |
| `POST` | `/api/stripe/checkout` | Start Student Pro subscription |
| `POST` | `/api/stripe/portal` | Manage billing |
| `POST` | `/api/stripe/webhook` | Stripe event handler |

---

## Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start development server |
| `npm run build` | Production build |
| `npm run start` | Start production server |
| `npm run lint` | Run ESLint |

---

## Contributing

Contributions are welcome! Feel free to open an issue or submit a pull request.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## License

Distributed under the **MIT License**. See [`LICENSE`](LICENSE) for more information.

---

<div align="center">

**Built by [tanuj201](https://github.com/tanuj201)**

[Live Demo](https://hackathon-chi-topaz.vercel.app) · [GitHub](https://github.com/tanuj201/Hackathon)

If CogniDrive helps your studies, consider giving the repo a star.

</div>
