# OpenWT Employee Co-Pilot

AI-powered chat assistant for OpenWT's Employee App — helps ~200 employees manage leave requests, policy questions, and document processing through natural conversation (Vietnamese/English).

## What is this?

A chat widget embedded directly into the existing Employee App (employee.openwt.vn). Employees can:

- Ask "Còn bao nhiêu ngày phép?" and get an instant answer
- Submit leave requests through chat instead of filling forms
- Upload prescriptions — AI reads and auto-fills the leave form
- Ask any HR policy question and get answers with source citations

**The Employee App stays unchanged** — the AI runs as a separate microservice, connected via a lightweight chat widget (3-5 lines of code added to the frontend).

## Tech Stack

| Component | Choice |
|-----------|--------|
| AI Agent | [Mastra](https://mastra.ai) + Hono (TypeScript) |
| Chat Widget | [CopilotKit](https://copilotkit.ai) (React) |
| LLM | Claude Haiku (chat) + Sonnet (Vision AI) |
| Knowledge Search | pgvector + OpenAI Embeddings |
| Database | PostgreSQL 16 |
| Cache | Redis |
| Deploy | Docker Compose (on-premise) |

## Project Status

**Phase: Planning complete, ready for approval**

| Milestone | Scope | Effort | Status |
|-----------|-------|--------|--------|
| M1 (MVP) | Leave balance, history, WFH, policy Q&A, submit leave, Vision AI | 2-3 weeks | Ready to start |
| M2 (Expand) | Device, Attendance, Profile, Buddy, Regulations, Training, Slack | ~2-3 weeks | After M1 demo |
| M3 (Workflows) | Task Manager (write operations) | ~1-2 weeks | After M2 |
| M4 (Intelligence) | Team Analytics, Knowledge Search, Proactive Notifications | ~7-10 weeks | After M3 |

## Documentation

All planning docs are in [`docs/`](docs/):

| Document | Description |
|----------|-------------|
| [docs/README.md](docs/README.md) | Documentation index and reading order |
| [M1 PRD](docs/core/m1-prd.md) | MVP product requirements, user flows, edge cases, tech decisions |
| [M1 Feasibility](docs/core/m1-feasibility.md) | Build sequence, cost estimate, Step 0 checklist |
| [Product Roadmap](docs/core/product-roadmap.md) | 4 milestones, 18 features, module coverage |
| [Research Report](docs/core/research-report.md) | Implementation details: tool schemas, auth, Docker, RAG |
| [AI Fundamentals](docs/core/ai-agent-fundamentals.md) | AI agent concepts: memory, RAG, guardrails |

## Cost

~$50-100/month (Claude API + OpenAI embeddings). On-premise deployment = $0 infrastructure.

Compare: SaaS alternatives (Leena AI, Moveworks) cost $400-1,600/month for 200 employees.

## Getting Started

> Pending management approval. See [M1 PRD](docs/core/m1-prd.md) and [Product Roadmap](docs/core/product-roadmap.md).

After approval:
1. **Phase 1 — Discovery (1 day):** Audit Employee App APIs, auth, frontend stack
2. **Phase 2 — Knowledge Base (1-2 days):** Export HR policies, normalize for RAG
3. **Phase 3 — MVP Build (2-3 weeks):** Build, test, demo

## License

Internal project — OpenWT proprietary.
