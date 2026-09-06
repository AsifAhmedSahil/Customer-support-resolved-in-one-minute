# OneMinute Support

AI Support Center Chatbot Builder — a B2B SaaS platform for building intelligent customer support agents.

## Overview

OneMinute Support is a Retrieval-Augmented Generation (RAG) customer support platform that enables businesses to build, train, and deploy AI-powered chatbots. It uses OpenAI for conversational AI with prompt engineering, context management, and knowledge retrieval to deliver accurate, context-aware responses. The platform includes scalable REST APIs, real-time conversation monitoring, and an embeddable widget for seamless website integration.

## Key Highlights

- RAG-based intelligent knowledge retrieval from uploaded sources
- Conversational AI workflows with prompt engineering and context management
- Scalable REST APIs for chatbot interactions
- Real-time conversation monitoring with human escalation
- Embeddable widget with zero-dependency JavaScript injection

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Next.js 16 (App Router) + React 19 |
| Language | TypeScript |
| Styling | Tailwind CSS v4 + shadcn/ui |
| Database | PostgreSQL (Neon) + Drizzle ORM |
| Auth | Scalekit (OAuth 2.0 / OIDC) |
| AI Engine | OpenAI GPT-4o / GPT-4o-mini |
| Web Scraping | Firecrawl |
| JWT | jose |

## Features

### AI & Knowledge
- **Knowledge Ingestion** — Add training data via website crawling, text input, or CSV upload
- **AI Summarization** — Automatically preprocess and summarize scraped content for optimal retrieval
- **Context Management** — Inject relevant knowledge into prompts; auto-summarize long conversations to stay within token limits
- **Prompt Engineering** — Structured system prompt with escalation protocols, tone enforcement, and context injection

### Chatbot Builder
- **Sections** — Organize knowledge into topic-specific sections with configurable tones (strict, neutral, friendly, empathetic)
- **Appearance Customization** — Configure chatbot colors, welcome message, and branding
- **Embeddable Widget** — Drop a `<script>` tag to add the chatbot to any website

### Dashboard & Operations
- **Live Conversation Monitoring** — View and reply to ongoing chats in real-time
- **Human Escalation** — AI automatically hands off to human agents when it can't resolve an issue
- **Team Collaboration** — Invite team members with role-based access via Scalekit
- **Overview Analytics** — Track knowledge sources, sections, and chat activity

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                     Client Layer                        │
│  Landing Page │ Dashboard │ Embeddable Widget (iframe)  │
└──────────┬──────────────────────────────┬───────────────┘
           │                              │
┌──────────▼──────────────────────────────▼───────────────┐
│                   API Layer (Next.js)                   │
│  /api/auth  /api/chat  /api/knowledge  /api/conversations│
│  /api/section  /api/widget  /api/team  /api/webhook     │
└──────────┬──────────────────────────────┬───────────────┘
           │                              │
┌──────────▼──────────┐  ┌────────────────▼───────────────┐
│   OpenAI GPT-4o     │  │     PostgreSQL (Neon)          │
│   GPT-4o-mini       │  │  8 tables via Drizzle ORM      │
│   Firecrawl         │  │  Knowledge │ Sections │ Users  │
└─────────────────────┘  └────────────────────────────────┘
```

## Project Structure

```
├── app/
│   ├── page.tsx                 # Landing page
│   ├── layout.tsx               # Root layout
│   ├── embed/                   # Embeddable chat widget
│   ├── dashboard/               # Authenticated dashboard
│   │   ├── chatbot/             # Chatbot playground & config
│   │   ├── sections/            # Section management
│   │   └── settings/            # Workspace & team settings
│   └── api/                     # REST API routes
│       ├── auth/                # OAuth (Scalekit)
│       ├── chat/                # Chat endpoints (test & public)
│       ├── knowledge/           # Knowledge CRUD
│       ├── section/             # Section CRUD
│       ├── conversations/       # Conversation management
│       ├── chatbot/             # Chatbot metadata
│       ├── team/                # Team management
│       ├── widget/              # Widget config & sessions
│       └── webhook/             # Webhook handlers
├── components/
│   ├── dashboard/               # Dashboard UI components
│   ├── landing/                 # Landing page sections
│   └── ui/                      # shadcn/ui components
├── db/
│   ├── client.ts                # Database connection (Neon)
│   └── schema.ts                # Drizzle schema (8 tables)
├── lib/
│   ├── openAI.ts                # OpenAI client + summarization
│   ├── scalekit.ts              # Scalekit SDK
│   ├── firecrawl.ts             # Firecrawl SDK
│   └── isAuthorized.ts          # Auth middleware
├── hooks/                       # React hooks
├── public/
│   └── widget.js                # Client-side widget loader
└── drizzle/                     # Database migrations
```

## REST API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/auth` | GET | Initiate OAuth login |
| `/api/auth/callback` | GET | OAuth callback |
| `/api/auth/logout` | GET | Clear session |
| `/api/metadata/store` | POST | Store business info |
| `/api/metadata/fetch` | GET | Fetch business info |
| `/api/knowledge/store` | POST | Add knowledge source |
| `/api/knowledge/fetch` | GET | List knowledge sources |
| `/api/section/create` | POST | Create a section |
| `/api/section/fetch` | GET | List sections |
| `/api/section/delete` | DELETE | Delete a section |
| `/api/chat/test` | POST | Test chat (authenticated) |
| `/api/chat/public` | POST | Public chat (widget) |
| `/api/chatbot/metadata/fetch` | GET | Fetch chatbot config |
| `/api/chatbot/metadata/update` | PUT | Update chatbot config |
| `/api/widget/session` | POST | Create widget JWT |
| `/api/widget/config` | GET | Fetch widget config |
| `/api/conversations` | GET | List conversations |
| `/api/conversations/[id]/messages` | GET | Fetch messages |
| `/api/conversations/[id]/reply` | POST | Human agent reply |
| `/api/overview` | GET | Dashboard stats |
| `/api/team/add` | POST | Invite team member |
| `/api/team/fetch` | GET | List team members |
| `/api/webhook/scalekit` | POST | Webhook handler |

## Database Schema

| Table | Purpose |
|-------|---------|
| `user` | User accounts |
| `metadata` | Organization & business info |
| `knowledge_source` | Training data (website/text/CSV) |
| `sections` | Topic configs with tone rules |
| `chatBotMetadata` | Chatbot appearance settings |
| `team_members` | Team invitations & roles |
| `conversation` | Chat sessions |
| `messages` | Individual chat messages |

## Getting Started

### Prerequisites

- Node.js 18+
- PostgreSQL database (Neon recommended)
- OpenAI API key
- Scalekit account
- Firecrawl API key

### Environment Variables

Create a `.env` file:

```env
DATABASE_URL=postgresql://...
OPENAI_API_KEY=sk-...
OPENAI_BASE_URL=
SCALEKIT_ENVIRONMENT_URL=https://...
SCALEKIT_CLIENT_ID=...
SCALEKIT_CLIENT_SECRET=
SCALEKIT_REDIRECT_URL=http://localhost:3000/api/auth/callback
SCALEKIT_WEBHOOK_SECRET=...
FIRECRAWL_API_KEY=fc-...
JWT_SECRET=your-random-secret
NEXT_PUBLIC_WEBSITE_URI=http://localhost:3000
```

### Installation

```bash
git clone https://github.com/your-username/oneminute-support.git
cd oneminute-support
npm install
npm run db:generate
npm run db:push
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

### Available Scripts

| Script | Description |
|--------|-------------|
| `npm run dev` | Start development server |
| `npm run build` | Production build |
| `npm run start` | Start production server |
| `npm run db:generate` | Generate Drizzle migrations |
| `npm run db:push` | Push schema to database |
| `npm run db:migrate` | Run pending migrations |
| `npm run db:studio` | Open Drizzle Studio |

## Embedding the Chatbot

Add to any website:

```html
<script
  src="https://your-domain.com/widget.js"
  data-id="YOUR_CHATBOT_ID"
></script>
```

The widget renders as a floating button that expands into a chat window.

## Deployment

```bash
npm run build
npm run start
```

For Vercel, connect your repo and configure environment variables in the dashboard.

## License

MIT
