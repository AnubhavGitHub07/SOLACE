# 🧠 SOLACE — Project Brain

> **Last Updated:** 2026-10-02
> **Status:** Phase 0 — Codebase Audit Complete, Architecture Locked

---

## 1. Project Identity

**Project Name:** SOLACE
**Tagline:** *A Silent Shield, A Strong Voice.*
**Hackathon Theme:** Best Use of Gemini API

**Core Product Statement:**

> An AI-powered safety platform built to help women facing harassment, abuse, or unsafe situations through discreet SOS communication, emotional support, legal guidance, and intelligent case analysis — powered by Gemini.

**Target Users:**
- **Primary:** Women who may be experiencing harassment, abuse, threats, or unsafe situations
- **Secondary:** Authorities who receive, review, and respond to SOS cases

**Prototype Constraints:**
- Prioritize a working end-to-end demo
- Local development is preferred
- NO unnecessary cloud infrastructure
- NO production deployment complexity
- NO over-engineering
- Focus on the experience judges will actually see

---

## 2. Final Architecture (LOCKED)

```
                     SOLACE
                       |
           +-----------+-----------+
           |           |           |
           v           v           v
       iOS APP     LANDING      DASHBOARD
       (Women)     WEBSITE      (Authority)
           |           |           |
           +-----------+-----------+
                       |
                       v
                 LOCAL BACKEND
                       |
              +--------+--------+
              |        |        |
              v        v        v
           Gemini   MongoDB   AI Avatar
```

### Component Map

| Component         | Technology                        | Purpose                                  |
|:------------------|:----------------------------------|:-----------------------------------------|
| iOS App           | Swift, SwiftUI                    | Primary user-facing safety app for women |
| Landing Website   | Next.js, TypeScript, Tailwind CSS | Public-facing product presentation       |
| Authority Dashboard | Next.js, TypeScript, Tailwind CSS | Case management for authorities        |
| Local Backend     | Python, FastAPI, Pydantic         | Central API / business-logic layer       |
| Gemini            | Google Gemini API                 | Central intelligence layer               |
| MongoDB           | MongoDB (+ Vector Search)         | Application database                     |
| AI Avatar         | React, R3F, Three.js, ElevenLabs  | Emotional support companion              |

---

## 3. Component Responsibilities

### A. iOS App — Women (NEW — Does Not Exist Yet)

- Safe onboarding flow
- Home / safety interface
- Discreet SOS creation
- Emergency message composition (situation, location, context)
- AI-powered assistance via Gemini
- AI companion / support experience
- Legal / safety guidance
- Clear emergency interaction flow
- Must feel like a real standalone native SwiftUI product

### B. Landing Website (TO BE CREATED — Current Frontend Is NOT This)

- Explain the problem SOLACE solves
- Explain how SOLACE works
- Showcase Gemini AI capabilities
- Explain discreet SOS, AI support, authority dashboard
- Polished product presentation
- Does NOT need complete application functionality

### C. Authority Dashboard (PARTIALLY EXISTS — Needs Redesign)

- Display SOS cases and case information
- Display AI-generated analysis, risk/severity
- Display location/context
- Case status and case management
- AI-assisted information for authorities
- Must be clearly designed for AUTHORITY users

### D. Local Backend (EXISTS — Needs Refactoring)

- REST API endpoints
- SOS processing pipeline
- Gemini integration (central)
- MongoDB CRUD operations
- Case management logic
- AI analysis orchestration
- Communication layer between iOS app and dashboard
- Steganography processing
- AI Avatar communication

### E. Gemini — Central Intelligence Layer (PARTIALLY EXISTS)

**Hackathon theme is "Best Use of Gemini API" — Gemini must be deeply integrated.**

```
                    GEMINI
                       |
       +---------------+---------------+
       |               |               |
       v               v               v
    SOS AI        AI COMPANION      LEGAL AI
       |               |               |
       v               v               v
 Situation         Emotional         Legal
 Analysis          Support           Guidance
       |
       v
 Authority AI
 Case Understanding
```

Gemini responsibilities:
- SOS message understanding & situation analysis
- Intent classification & risk/severity analysis
- Structured emergency information extraction
- Conversational AI for emotional support
- Legal assistance & RAG-based legal reasoning
- Case summarization for authorities
- Embeddings / vector-based similarity
- AI-generated recommendations / context

**Rule:** Every Gemini integration must solve a real product problem. No artificial usage.

### F. MongoDB

- Users, SOS cases, emergency reports
- Case metadata and AI-generated structured information
- Complaint/case records
- Embeddings where required
- Vector Search for semantic similarity (only where valuable)

### G. AI Avatar (EXISTS — Needs SOLACE Integration)

- Emotional support companion
- Voice-based communication
- Facial expressions and lip synchronization
- Powered by Gemini (replace OpenAI/Vertex AI references)
- Must feel like a SOLACE feature, not an unrelated demo

---

## 4. Current Codebase Audit

### Repository Structure (As Found)

```
SOLACE/
├── backend/                    # Python FastAPI backend
│   ├── main.py                 # FastAPI app with all endpoints
│   ├── db.py                   # MongoDB connection & operations
│   ├── prompts.py              # LLM prompt templates
│   ├── schema.py               # Pydantic models
│   ├── logger.py               # Custom logging
│   ├── requirements.txt        # Python dependencies
│   ├── docs/                   # Legal PDF documents
│   └── utils/
│       ├── ai_assitant.py      # CLI-based AI assistant (hardcoded keys!)
│       ├── common.py           # Utility functions
│       ├── embedding.py        # Gemini text embeddings + vector search
│       ├── regex_ptr.py        # Regex text extraction
│       ├── steganography.py    # LSB image steganography
│       ├── text_llm.py         # Gemini + Groq LLM calls
│       └── twitter.py          # Twitter/X posting
├── frontend/                   # Next.js web frontend (serves BOTH user + dashboard)
│   ├── src/
│   │   ├── app/
│   │   │   ├── page.tsx        # Home page (Header component)
│   │   │   ├── layout.tsx      # Root layout (Clerk + Navbar)
│   │   │   ├── dashboard/      # Admin dashboard (Clerk role check)
│   │   │   ├── create-post/    # SOS post creation
│   │   │   ├── lawbot/         # Legal bot page
│   │   │   ├── therapybot/     # Therapy bot page
│   │   │   ├── post/           # Post detail view
│   │   │   ├── sign-in/        # Clerk sign-in
│   │   │   ├── sign-up/        # Clerk sign-up
│   │   │   ├── user/           # User profile
│   │   │   └── api/            # Next.js API routes
│   │   ├── components/         # 15 React components + ui/
│   │   └── lib/                # Utility (utils.ts)
│   └── middleware.ts           # Clerk auth middleware
├── ai-avatar/
│   ├── ai-avatar-backend/      # Express.js server (Node.js)
│   │   ├── index.js            # Chat endpoint + ElevenLabs TTS + lip sync
│   │   ├── audios/             # Generated audio files
│   │   └── package.json        # (name: "r3f-virtual-girlfriend-backend")
│   └── ai-avatar-frontend/     # Vite + React Three Fiber
│       ├── src/
│       │   ├── components/     # Avatar.jsx, Experience.jsx, UI.jsx
│       │   ├── App.jsx
│       │   └── main.jsx
│       └── public/
│           ├── models/         # 3D GLB models
│           └── animations/     # Animation files
└── docs/
    └── images/                 # Documentation images
```

### Legacy Classification

| Item | Status | Notes |
|:-----|:-------|:------|
| `backend/main.py` — FastAPI core | **REFACTOR** | Solid foundation but needs cleanup. AWS/S3/Bedrock deps must be removed. Endpoints need restructuring for SOLACE architecture. |
| `backend/db.py` — MongoDB connection | **REFACTOR** | Works but references "SheBuilds" database name. Needs rename to "solace". |
| `backend/prompts.py` — LLM prompts | **REFACTOR** | Good prompt templates. Need SOLACE branding (remove "Platform X"). |
| `backend/schema.py` — Pydantic models | **REFACTOR** | PostInfo model is good, needs expansion for SOLACE SOS flows. |
| `backend/utils/steganography.py` — LSB encoding | **KEEP** | Core SOLACE feature. Clean, working implementation. |
| `backend/utils/text_llm.py` — Gemini/Groq | **REFACTOR** | Gemini integration is useful. Groq/Gemma is a secondary provider — evaluate whether to keep. |
| `backend/utils/embedding.py` — Gemini embeddings | **KEEP** | Uses Gemini text-embedding-004. Core for vector search. |
| `backend/utils/ai_assitant.py` — CLI AI assistant | **REMOVE** | Hardcoded API keys, standalone script, not integrated. Functionality will be handled by Gemini in backend. |
| `backend/utils/twitter.py` — Twitter posting | **DEFER** | SOS distribution channel. Not critical for MVP demo. |
| `backend/utils/common.py` — Utilities | **KEEP** | Standard utility functions. |
| `backend/utils/regex_ptr.py` — Regex extraction | **REFACTOR** | Fragile regex parsing. Gemini should handle structured extraction instead. |
| AWS/S3/Bedrock integration | **REMOVE** | Image gen via Bedrock, storage via S3. Not needed for prototype. |
| `frontend/` — Next.js web app | **REFACTOR → SPLIT** | Currently serves BOTH user and authority. Needs splitting into Landing Website + Authority Dashboard. |
| Clerk authentication | **DEFER/REPLACE** | Currently used for auth. Evaluate whether needed for prototype or if simpler auth suffices. |
| `ai-avatar/ai-avatar-backend/` | **REFACTOR** | Express server with Vertex AI + ElevenLabs. Hardcoded Windows paths for Rhubarb. Needs platform-agnostic fixes + Gemini migration. |
| `ai-avatar/ai-avatar-frontend/` | **KEEP** | React Three Fiber 3D avatar. Core SOLACE feature. |
| OpenAI dependency (avatar backend) | **REMOVE** | Fallback reference, not actively used but imported. Replace with Gemini. |
| Vertex AI usage (avatar backend) | **REPLACE** | Currently uses `@google-cloud/vertexai`. Should use `@google/generative-ai` for Gemini API. |

### Critical Conflicts Found

| # | Conflict | Current State | Target State |
|:--|:---------|:-------------|:-------------|
| 1 | **No iOS app exists** | Everything is web-based | Primary product must be native SwiftUI iOS |
| 2 | **Frontend serves dual purpose** | Single Next.js app for users AND admins | Separate Landing Website + Authority Dashboard |
| 3 | **Database named "SheBuilds"** | `db_client["SheBuilds"]` | Should be `"solace"` |
| 4 | **AWS dependencies embedded** | S3, Bedrock, boto3 in backend | Remove — use local or Gemini-native solutions |
| 5 | **Multiple AI providers** | Gemini + Groq/Gemma + OpenAI + Vertex AI | Consolidate to Gemini API as primary |
| 6 | **Avatar backend hardcoded Windows paths** | `E:\\demo\\...\\rhubarb.exe` | Platform-agnostic paths, macOS-compatible |
| 7 | **Avatar package named "virtual-girlfriend"** | `r3f-virtual-girlfriend-backend` | Rename to SOLACE-aligned naming |
| 8 | **Clerk auth tightly coupled** | Clerk middleware + providers throughout | Evaluate: keep for prototype or simplify |
| 9 | **No structured SOS → Dashboard flow** | Endpoints exist separately | Need end-to-end: iOS → Backend → MongoDB → Dashboard |
| 10 | **Prompts reference "Platform X"** | Legacy branding in prompts | Must reference SOLACE |
| 11 | **ai_assitant.py has hardcoded keys** | `self.elevenlabs_api_key = "s"` | Must use env vars, file should be removed |

---

## 5. Gemini-First Strategy

### Principle

Gemini is NOT just another API. It is the **central intelligence layer** of SOLACE.

### Integration Points

| Feature | Gemini Role | API Used |
|:--------|:------------|:---------|
| SOS message expansion | Understands brief crisis input → structured report | `gemini-2.0-flash` |
| Situation analysis | Risk assessment, severity classification | `gemini-2.0-flash` |
| Structured extraction | JSON extraction of victim/culprit/location info | `gemini-2.0-flash` |
| Text embeddings | Culprit similarity, legal doc retrieval | `text-embedding-004` |
| AI companion chat | Therapeutic, empathetic conversation | `gemini-2.0-flash` |
| Legal guidance | RAG over legal documents + plain-language answers | `gemini-2.0-flash` |
| Case summarization | Authority-facing case digests | `gemini-2.0-flash` |
| Avatar intelligence | Powers avatar conversation responses | `gemini-2.0-flash` |

### What We Should Be Able to Say in the Demo

> "Gemini acts as the intelligence layer connecting the user's safety experience, AI companion, and authority-side case understanding."

---

## 6. Core Demo Flow (End-to-End Target)

```
Woman
  ↓
iOS App
  ↓
Trigger SOS
  ↓
Describe situation
  ↓
Gemini understands situation
  ↓
Safety / risk / intent analysis
  ↓
Generate discreet SOS
  ↓
Encode/secure emergency information
  ↓
Send/share SOS
  ↓
Local Backend
  ↓
MongoDB
  ↓
Authority Dashboard
  ↓
Authority views:
  ├── Case details
  ├── Situation summary
  ├── AI analysis
  ├── Risk/severity
  ├── Location/context
  └── Related cases (vector search)
```

---

## 7. Implementation Phases

| Phase | Description | Status |
|:------|:------------|:-------|
| **Phase 1** | Understand and stabilize existing backend | 🟡 Audit Complete |
| **Phase 2** | Make FastAPI + MongoDB + Gemini work reliably | ⬜ Not Started |
| **Phase 3** | Create native SwiftUI iOS application | ⬜ Not Started |
| **Phase 4** | Connect iOS → Local Backend → MongoDB/Gemini | ⬜ Not Started |
| **Phase 5** | Build/connect Authority Dashboard | ⬜ Not Started |
| **Phase 6** | Integrate and adapt AI Avatar | ⬜ Not Started |
| **Phase 7** | Build/refine Landing Website | ⬜ Not Started |
| **Phase 8** | Integrate complete SOS flow end-to-end | ⬜ Not Started |
| **Phase 9** | Polish UI/UX and SOLACE visual identity | ⬜ Not Started |
| **Phase 10** | Prepare final hackathon demo flow | ⬜ Not Started |

---

## 8. Legacy Code Strategy

When encountering existing code, classify as:

| Action | When to Apply |
|:-------|:-------------|
| **KEEP** | Working, aligned with SOLACE, no changes needed |
| **REFACTOR** | Useful but needs adaptation for SOLACE architecture |
| **REPLACE** | Functionality needed but implementation is wrong |
| **REMOVE** | Conflicts with SOLACE or is dead code |
| **DEFER** | Useful but not critical for MVP demo |

**Rules:**
- Source code is the source of truth, NOT README documentation
- Understand dependencies before making major changes
- Do NOT rewrite working components without a reason
- Do NOT introduce duplicate implementations

---

## 9. Decision-Making Framework

For every implementation decision, ask:

1. Does this align with SOLACE?
2. Does this support the iOS + Landing + Dashboard architecture?
3. Does this improve the core safety experience?
4. Does this make meaningful use of Gemini?
5. Does this help the hackathon demo?
6. Is this necessary?
7. Can we implement it more simply?

If the answer to most is NO → do not introduce the change.

---

## 10. Environment Variables Required

```env
# Gemini
GEMINI_API_KEY=

# MongoDB
MONGO_ENDPOINT=

# ElevenLabs (Avatar)
ELEVEN_LABS_API_KEY=

# Legacy (to be removed)
# AWS_ACCESS_KEY_ID=
# AWS_SECRET_ACCESS_KEY=
# AWS_REGION=
# S3_BUCKET_NAME=
# GROQ_API_TOKEN=
# TWITTER_CONSUMER_KEY=
# TWITTER_CONSUMER_SECRET=
# TWITTER_ACCESS_TOKEN=
# TWITTER_ACCESS_TOKEN_SECRET=
# TWITTER_BEARER_TOKEN=
# OPENAI_API_KEY=
# CLERK_SECRET_KEY=
# NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
```

---

## 11. Key Technical Decisions Log

| Date | Decision | Rationale |
|:-----|:---------|:----------|
| 2026-10-02 | Architecture locked: iOS + Landing + Dashboard + Local Backend | Hackathon target architecture |
| 2026-10-02 | Gemini as sole primary AI provider | Hackathon theme: "Best Use of Gemini API" |
| 2026-10-02 | AWS/S3/Bedrock to be removed | Unnecessary cloud infra for prototype |
| 2026-10-02 | Steganography module preserved | Core differentiating feature |
| 2026-10-02 | Avatar to be migrated from Vertex AI → Gemini API | Consolidate AI providers |

---

## 12. Maintenance Rules

- Update this file when a **major** architectural decision is made
- Do NOT rewrite after every small code change
- Keep concise enough to remain useful
- **Always read this file before starting a new task**
