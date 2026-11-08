# MedIntel AI — Clinical Knowledge & Medical RAG Platform

> Enterprise-grade, multi-tenant Medical Retrieval-Augmented Generation (RAG) platform designed for clinical guidelines, hospital departments, scientific publications, and healthcare knowledge retrieval.

---

## 🏥 Overview

**MedIntel AI** enables clinical institutions, research teams, and individual medical practitioners to ingest medical documents (PDFs, clinical guidelines, research studies) and query them through an AI assistant with semantic citations, source verification, and multi-tenant organization isolation.

The platform is built with a **Dual-Mode Architecture**:
1. **Zero-Config Offline Simulation Mode (Default):** Runs immediately in browser storage with realistic clinical demo data (Mayo Clinic Cardiology, Stanford Genomics), interactive onboarding, live medical document ingestion, client-side semantic search, and AI assistant reasoning.
2. **Production Cloud Mode (Supabase + pgvector + Gemini):** Simply supply `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` in `.env` along with `GEMINI_API_KEY` to connect to a live Supabase PostgreSQL database with row-level security (RLS) and pgvector embeddings.

---

## ✨ Features

- **Multi-Tenant Clinical Workspaces:** Institutional workspaces (hospitals, research departments) and isolated personal practitioner workspaces.
- **Onboarding Flow:** New users verify their account and choose between creating an institutional clinic, joining an existing workspace via key, or launching a personal workspace.
- **Document Management & Ingestion:** Upload clinical guidelines, trial protocols, and research PDFs with metadata extraction, chunking, and tag categorization.
- **AI Medical Assistant:** Conversational clinical agent featuring verifiable citations, citation pills linking to source chunks, clinical reasoning, and query suggestions.
- **Semantic & Keyword Medical Search:** High-precision search across all indexed clinical literature with similarity scoring and relevance filtering.
- **Role-Based Access Control (RBAC):** `Owner`, `Admin`, `Member`, and `Viewer` permission levels strictly partitioned per organization.
- **Audit Logging & Compliance:** Tracks clinical document uploads, queries, and team membership events.

---

## 🛠️ Tech Stack

- **Frontend:** React 19, TypeScript, Vite 6, Tailwind CSS v4, Motion (Framer Motion), Lucide Icons
- **Document Processing:** PDF.js (`pdfjs-dist`) for clinical PDF parsing
- **AI Integration:** `@google/genai` (Google Gemini 2.5 Flash / Flash-Lite / Pro)
- **Database & Backend:**
  - Production: Supabase PostgreSQL (`@supabase/supabase-js`), `pgvector` extension, Row-Level Security (RLS)
  - Development / Demo: Built-in local state simulator (`supabaseSim.ts`) with persistent browser storage
- **Tooling:** ESLint, TypeScript compiler (`tsc --noEmit`), Vite build system

---

## 🚀 Quick Start

### 1. Prerequisites
- **Node.js** v18+ or v20+
- **npm** or **pnpm** or **bun**

### 2. Installation
```bash
# Clone repository
git clone https://github.com/YOUR_USERNAME/medintel-ai.git
cd medintel-ai

# Install dependencies
npm install
```

### 3. Environment Configuration (Optional)
The application works immediately in simulation mode without any keys. To connect real cloud services:

```bash
cp .env.example .env
```

Edit `.env`:
```env
# Google Gemini API Key for real-time generative clinical answers
GEMINI_API_KEY="your-gemini-api-key"

# Supabase project credentials (leave blank for local simulator mode)
VITE_SUPABASE_URL="https://your-project.supabase.co"
VITE_SUPABASE_ANON_KEY="your-anon-key"
```

### 4. Run Development Server
```bash
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) in your browser.

### 5. Production Build & Lint
```bash
# Validate TypeScript types
npm run lint

# Compile production bundle
npm run build

# Preview production build locally
npm run preview
```

---

## 🗄️ Database Migrations (Supabase)

All SQL schema definitions, foreign keys, pgvector indices, and Row-Level Security policies are located in `supabase/migrations/`:

| Migration | Purpose |
|---|---|
| `001_initial_schema.sql` | Organizations, profiles, memberships, audit logs |
| `002_documents.sql` | Document registry, chunks, and metadata schemas |
| `003_chat.sql` | Chat sessions, messages, and citation schemas |
| `004_storage.sql` | Supabase storage bucket configuration for clinical PDFs |
| `005_vector_search.sql` | `pgvector` extension setup, embeddings column, and match RPC |
| `006_rls_policies.sql` | Multi-tenant RLS isolation policies across all tables |
| `007_auth_profile_trigger.sql` | Database trigger to sync Supabase Auth users to `profiles` |

To apply to your Supabase project:
1. Link your Supabase project with `supabase link --project-ref your-project-id`.
2. Run `supabase db push`, or paste the SQL files sequentially into the Supabase SQL Editor.

---

## 📤 Uploading to GitHub

Follow these steps to initialize git and push this project to GitHub:

```bash
# 1. Initialize git (if not already initialized)
git init

# 2. Add files to staging
git add .

# 3. Create your initial commit
git commit -m "feat: initial commit for MedIntel AI platform"

# 4. Rename default branch to main
git branch -M main

# 5. Add your GitHub remote repository
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git

# 6. Push to GitHub
git push -u origin main
```

---

## 📁 Project Structure

```
├── docs/                      # Architectural and technical documentation
│   ├── api.md
│   ├── architecture.md
│   └── database.md
├── src/
│   ├── components/            # React UI components & view controllers
│   │   ├── AIAssistantView.tsx
│   │   ├── AppLayout.tsx
│   │   ├── AuthView.tsx
│   │   ├── DashboardView.tsx
│   │   ├── DocumentsView.tsx
│   │   ├── MedicalSearchView.tsx
│   │   ├── OnboardingView.tsx
│   │   └── SettingsView.tsx
│   ├── lib/                   # API clients, Gemini SDK, Supabase dual-driver
│   │   ├── gemini.ts
│   │   ├── supabase.ts
│   │   └── supabaseSim.ts
│   ├── App.tsx                # Main state controller & auth router
│   ├── main.tsx               # App entrypoint
│   └── index.css              # Global styles (Tailwind CSS v4)
├── supabase/
│   ├── migrations/            # Production PostgreSQL SQL migrations
│   └── seed/                  # Seed data
├── .env.example               # Example environment variable template
├── .gitignore                 # Standard git exclusions (node_modules, secrets)
├── index.html                 # HTML shell
├── package.json               # Dependencies and scripts
├── tsconfig.json              # TypeScript configuration
└── vite.config.ts             # Vite configuration
```

---

## 📄 License
MIT License. Built for clinical and research exploration.
