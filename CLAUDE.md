# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

AI Hero is an open-source educational platform for AI engineering created by Matt Pocock. The repository contains examples, exercises, articles, and a complete course for learning AI engineering concepts from zero to advanced.

## Architecture

### Repository Structure

- **`courses/`** - **PRIMARY CODEBASE** - Production-ready applications and complete course materials
  - `01-deepsearch-in-typescript/` - Main 9-day course building an agentic search engine
  - `00-apps/` - 6 progressive application builds (day-1-app through final-app)
- **`examples/`** - Standalone code samples demonstrating specific AI concepts
- **`articles/`** - Educational content and documentation
- **`internal/`** - Build scripts, development tools, and repository management utilities

### Main Course Applications Architecture

The real codebase lives in `courses/01-deepsearch-in-typescript/00-apps/`, which contains 6 progressive Next.js applications:

**Application Structure:**
```
src/
├── app/                 # Next.js App Router
├── components/          # Reusable UI components  
├── server/             # Backend logic
│   ├── auth/           # Authentication config
│   ├── db/             # Database queries/schema
│   └── redis/          # Caching layer
├── [feature].ts        # Feature modules (search, agents, etc.)
└── types.ts           # TypeScript definitions
```

### Key Technologies (Main Applications)

**Core Stack:**
- **Next.js 15** - React framework with App Router
- **TypeScript** - Primary language
- **Vercel AI SDK v4** - LLM integration and streaming
- **Google Generative AI** - Primary LLM provider

**Database & Storage:**
- **PostgreSQL** with **pgvector** extension for embeddings
- **Drizzle ORM** - Type-safe database operations
- **Redis** - Caching and rate limiting
- **Auth.js (NextAuth.js)** - Discord OAuth authentication

**AI/ML Stack:**
- **Langfuse** - LLM observability and tracing
- **Evalite** - Custom evaluation framework
- **Serper API** - Web search integration
- **Cheerio** - Web scraping capabilities

## Development Commands

### Course Application Commands (Main Codebase)

Navigate to any app directory in `courses/01-deepsearch-in-typescript/00-apps/[app-name]/`:

```bash
# Development
pnpm dev                # Next.js dev server
pnpm build             # Production build
pnpm start             # Production server

# Database Management
pnpm db:generate       # Generate Drizzle migrations
pnpm db:migrate        # Run database migrations  
pnpm db:push          # Push schema changes
pnpm db:studio        # Open Drizzle Studio (database GUI)

# Infrastructure Setup
./start-database.sh    # Start PostgreSQL with pgvector in Docker
./start-redis.sh       # Start Redis instance in Docker

# Evaluation & Testing
pnpm evals            # Run Evalite evaluations
pnpm check            # Lint + typecheck combined

# Code Quality
pnpm lint             # ESLint
pnpm typecheck        # TypeScript compiler
pnpm format:write     # Prettier formatting
```

### Repository-Level Commands

From the root directory:

```bash
# Install dependencies
pnpm install

# Run linting and type checking
pnpm run lint

# Run a specific example (e.g., vercel-ai-sdk example 01)
pnpm run example v 01

# Watch all examples with evaluation
pnpm run all-examples

# Development server for articles
pnpm run articles:dev
```

### Internal Development

```bash
# Reorder examples and lint
pnpm run reorder

# Add gitkeep files
pnpm run gitkeep

# Add new lesson
pnpm run add-lesson

# Update shortlinks
pnpm run shortlinks:update
```

## Key Patterns

### Course Application Architecture Patterns

**Agentic Search Loop:**
- **SystemContext** - Maintains conversation state and reasoning history
- **Tool-based Design** - Modular tools for search, scraping, summarization that compose together
- **Streaming Responses** - Real-time UI updates using Vercel AI SDK streaming
- **Multi-step Reasoning** - Agent loop pattern in `run-agent-loop.ts`

**Database Schema:**
```typescript
// Authentication (NextAuth.js compatible)
users, accounts, sessions, verificationTokens

// Application Data  
chats: { id, userId, title, createdAt, updatedAt }
messages: { id, chatId, role, parts, annotations, order }
```

### Environment Setup

**Course Applications require:**
```bash
# Database
DATABASE_URL=postgresql://...
REDIS_URL=redis://...

# AI Services
GOOGLE_GENERATIVE_AI_API_KEY=your-key
SERPER_API_KEY=your-key

# Authentication
AUTH_DISCORD_ID=your-discord-app-id
AUTH_DISCORD_SECRET=your-discord-secret

# Observability
LANGFUSE_SECRET_KEY=your-key
LANGFUSE_PUBLIC_KEY=your-key
```

**Examples require:**
```bash
OPENAI_API_KEY=your-key
# OR
ANTHROPIC_API_KEY=your-key
```

### Testing with Evalite

The repository uses Evalite for evaluating AI systems. Both examples and course applications include evaluation files that test LLM outputs against expected behaviors using deterministic and LLM-as-judge approaches.

## Writing Style Guidelines

When working with articles or documentation:
- Use conversational but technical tone
- Focus on practical applications over theory
- Include Mermaid diagrams for complex concepts (flowcharts only, no custom styling)
- Structure content with clear headings and actionable conclusions
- Target audience: developers transitioning to AI engineering

## Course Format

Course materials follow a specific structure:
- **Steps to complete** - Addressed to LLMs for automation
- **The exercise is finished when** - Clear exit conditions
- **Not required yet** - Prevents over-implementation

## TypeScript Configuration

- Uses strict TypeScript with `noUncheckedIndexedAccess`
- ESNext target with NodeNext modules
- Examples directory excluded from main TypeScript project
- JSX configured for React

## Package Management

- Uses `pnpm` as package manager (version 8.15.6)
- Dependencies include AI SDK packages, React, testing frameworks
- Prettier configured with 55-character print width