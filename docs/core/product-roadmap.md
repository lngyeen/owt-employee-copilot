# OpenWT Employee Co-Pilot — Product Roadmap

*Last updated: 2026-04-07*

## Vision

Transform OpenWT's Employee App from a passive portal into an **AI-native workspace** where employees manage leave, devices, training, and company knowledge through a single conversational interface — reducing clicks, eliminating repetitive HR questions, and enabling proactive assistance.

## Architecture Principle

The Mastra + CopilotKit infrastructure built in Milestone 1 is **reusable for all subsequent milestones**. Each new feature follows the same pattern:

1. Add new **Tool(s)** to the agent (API integrations)
2. Expand **RAG knowledge base** (if policy/doc Q&A is needed)
3. Add **Mastra workflow** (if write operations or multi-step flows)

Same brain, more arms. No re-architecture needed between milestones.

## Related Documents

| Document | Purpose |
|----------|---------|
| [m1-prd.md](m1-prd.md) | Product Requirements for Milestone 1 (MVP) |
| [research-report.md](research-report.md) | Technical research (10 sections) |
| [m1-prd.md — Decisions Log](m1-prd.md#decisions-log) | 5 key tech decisions with rationale |
| [m1-feasibility.md](m1-feasibility.md) | Phase 1 feasibility, build order, cost estimate |

---

## 1. Roadmap Overview

```mermaid
graph TD
    T1["Milestone 1: Core<br/><i>PRD complete, ready to implement</i>"]
    T1a["Policy KB Setup (1-2 days)"]
    T1b["MVP: leave + policy + Vision AI (2-3 weeks)"]

    T2["Milestone 2: Expand<br/><i>Read tools for all remaining modules + Slack</i>"]
    T2a["Device Info + IT Support"]
    T2b["Attendance Overview"]
    T2c["Profile Lookup"]
    T2d["Buddy Lookup"]
    T2e["Company Regulations Q&A"]
    T2f["Training Discovery"]
    T2g["Slack Bot"]

    T3["Milestone 3: Workflows<br/><i>Write operations beyond leave</i>"]
    T3a["Task Manager Co-Pilot"]

    T4["Milestone 4: Intelligence<br/><i>Cross-module, dashboard, proactive</i>"]
    T4a["Team Analytics (Managers)"]
    T4b["Semantic Knowledge Search"]
    T4c["Proactive AI Notifications"]

    T1 --> T1a & T1b
    T1 ==> T2
    T2 --> T2a & T2b & T2c & T2d & T2e & T2f & T2g
    T2 ==> T3
    T3 --> T3a
    T3 ==> T4
    T4 --> T4a & T4b & T4c

    style T1 fill:#e8f5e9,stroke:#4caf50,stroke-width:2px
    style T2 fill:#e3f2fd,stroke:#2196f3,stroke-width:2px
    style T3 fill:#fff3e0,stroke:#ff9800,stroke-width:2px
    style T4 fill:#fce4ec,stroke:#e91e63,stroke-width:2px
```

---

## 2. Milestone Details

### Milestone 1: Core — Vacation Co-Pilot

> **Status:** PRD complete, ready to implement. Full details in [m1-prd.md](m1-prd.md), build order in [m1-feasibility.md](m1-feasibility.md).

Establishes core AI infrastructure: Mastra + Hono backend, CopilotKit widget, Claude API, PostgreSQL + Redis, RAG pipeline, Docker Compose deployment. All subsequent milestones build on this foundation.

| Phase | Deliverables | Effort |
|-------|-------------|--------|
| **Phase 1: Discovery** | Audit Employee App: API endpoints, auth mechanism, frontend stack, deployment | 1 day |
| **Phase 2: Knowledge Base** | Export HR policies from wiki + audit BE/FE source code for integration approach | 1-2 days |
| **Phase 3: MVP Build** | Balance lookup, leave/WFH history, policy Q&A, submit leave, Vision AI, audit logging | 2-3 weeks |

---

### Milestone 2: Expand — Read Tools for All Modules + Slack

> Read-only lookups across all remaining Employee App modules. Same AI engine, just add new tools and RAG content. No new write operations.

| Feature | User asks | Implementation | Module | Effort |
|---------|----------|---------------|--------|--------|
| **Device Info Query** | "Laptop serial number?" / "Devices assigned to me?" | Tool `getDeviceInfo(userId)` | Device | 0.5 day |
| **Device IT Support** | "MacBook slow, who to contact?" / "Warranty status?" | Enhance `getDeviceInfo` + RAG over IT policies | Device | 2-3 days |
| **Attendance Overview** | "Hôm nay check-in lúc mấy giờ?" / "Tuần này làm bao nhiêu giờ?" | Tool `getAttendanceOverview(userId, dateRange)` | Attendance | 1-2 days |
| **Profile Lookup** | "Skills của tôi?" / "Employment history?" / "Certifications?" | Tools `getProfile`, `getSkills`, `getEducation`, `getCertifications` | Profile | 2-3 days |
| **Buddy Lookup** | "Who is my buddy?" / "My buddees?" | Tools `getBuddyInfo`, `getMyBuddees` — read-only | Buddy | 1-2 days |
| **Company Regulations Q&A** | Any company regulation (working hours, performance, salary...) | RAG over Company Regulations content (already structured in app) | Company Regs | 1-2 days |
| **Training Discovery** | "Trainings của tôi?" / "TOEIC Test status?" | Tool `getMyTrainings(userId)` | Training | 2-3 days |
| **Slack Bot** | Same AI, accessible from Slack | Slack webhook → shared Mastra backend | External | 3-5 days |

**Milestone 2 total: ~2-3 weeks.** All features reuse Milestone 1 infra — only new tools, RAG content, and Slack integration needed.

---

### Milestone 3: Workflows — Write Operations Beyond Leave

> Write operations that modify data in Employee App (beyond leave requests already in Milestone 1). Requires human-in-the-loop confirmation patterns from Milestone 1.

| Feature | User asks / does | Implementation | Module | Effort |
|---------|-----------------|---------------|--------|--------|
| **Task Manager Co-Pilot** | "My tasks?" / "Mark X done" / "What's overdue?" / "Tasks with priority High?" | Tools `getTasks`, `updateTaskStatus` + filtering by status/priority/label. Write op: update task status requires HITL confirm | Tasks | 5-7 days |

**Milestone 3 total: ~1-2 weeks.** Reuses HITL confirmation patterns from Milestone 1 submit leave flow.

---

### Milestone 4: Intelligence

> Cross-module orchestration, proactive behavior, role-based features. Each feature is 2-4 weeks.

| Feature | Description | Key Dependency | Effort (weeks) |
|---------|------------|---------------|----------------|
| **Team Analytics (Managers)** | Team leave summaries. RBAC already supported — see [research-report.md](research-report.md) §9.4 | Aggregation APIs + dashboard UI | 2-3 weeks |
| **Semantic Knowledge Search** | Natural language search across ALL internal docs | Hierarchical RAG + hybrid search | 2-3 weeks |
| **Proactive AI Notifications** | "Leave expires in 2 weeks" / "Overtime detected" / "Warranty expiring" | Scheduled Mastra workflows + push routing | 3-4 weeks |

**Milestone 4 total: ~7-10 weeks.** Requires new UI components (dashboard) and advanced RAG techniques. Can be parallelized if multiple developers are available.

---

## 3. Feature Matrix

| # | Feature | M | Fitness | AI Readiness | Interface |
|---|---------|------|---------|--------------|-----------|
| 1 | Vacation Balance Lookup | 1 | **Chat** | Ready | Pure chat |
| 2 | Leave History Query | 1 | **Hybrid** | Ready | Chat card + "View all" link |
| 3 | HR Policy Q&A (RAG) | 1 | **Chat** | Ready | Chat with source citation |
| 4 | WFH History Lookup | 1 | **Hybrid** | Ready | Chat card + "View all" link |
| 5 | Submit Leave Request | 1 | **Hybrid** | Careful | Chat → summary → confirm → submit |
| 6 | Vision AI Documents | 1 | **Hybrid** | Careful | Upload (optional) → preview → confirm |
| 7 | Device Info Query | 2 | **Chat** | Ready | Pure chat |
| 8 | Device IT Support | 2 | **Hybrid** | Careful | Chat FAQ → IT ticket if unresolved |
| 9 | Attendance Overview | 2 | **Chat** | Ready | Pure chat |
| 10 | Profile Lookup | 2 | **Chat** | Ready | Pure chat |
| 11 | Buddy Lookup | 2 | **Chat** | Ready | Pure chat |
| 12 | Company Regulations Q&A | 2 | **Chat** | Ready | Chat with source citation |
| 13 | Training Discovery | 2 | **Chat** | Ready | Pure chat |
| 14 | Slack Bot | 2 | **Chat** | Ready | Slack native |
| 15 | Task Manager | 3 | **Hybrid** | Careful | Chat queries + updates (write) |
| 16 | Team Analytics | 4 | **UI-first** | Not suited | Dashboard. Chat as optional entry |
| 17 | Knowledge Search | 4 | **Chat** | Careful | Chat with source links |
| 18 | Proactive Notifications | 4 | **Chat** | Not suited | Chat nudge / Slack DM |

**Summary:** 11 Chat / 6 Hybrid / 1 UI-first / 0 No-go (18 features: 6 MVP + 8 Milestone 2 + 1 Milestone 3 + 3 Milestone 4)

**Multi-Language:** Vietnamese/English support is built-in from Phase 1 — Claude handles both natively. System prompt instruction "respond in same language as user" is all that's needed. No separate milestone or effort required.

### AI Readiness Legend

| Rating | Meaning | Criteria |
|--------|---------|----------|
| **Ready** | Build now with AI agent | Read-only, low risk, text response sufficient |
| **Careful** | AI agent viable with safeguards | Write operations or data extraction — requires human confirmation step |
| **Not suited** | AI agent is wrong primary interface | High-stakes decisions, data-heavy dashboards, sensitive data, or proactive push — use traditional UI with optional AI entry point |

---

## 4. UX Design Principles

### Chat Fitness Ratings

| Rating | Meaning | Best Interface |
|--------|---------|---------------|
| **Chat** | Faster than clicking through pages | Pure chat response |
| **Hybrid** | Chat initiates, UI completes. AI pre-fills, user confirms | Chat + Agentic UI |
| **UI-first** | Traditional UI is better. Chat adds marginal value | Dashboard/table with optional chat entry |
| **No-go** | Security risk or UX disaster in chat | Traditional UI with MFA |

### AI Limitations

| Limitation | Rule |
|------------|------|
| **Cognitive Load** | If visual selection > verbal description → Hybrid |
| **Data Density** | If results > 5 items → card/table outside chat or link to page |
| **High-Stakes** | Financial, delete, admin actions → No-go for chat-only |
| **Strict Validation** | Never let AI calculate — AI calls deterministic code, displays result |

### Hybrid Strategy

```mermaid
graph LR
    subgraph Chat["CHAT does"]
        C1["Quick lookups (balance)"]
        C2["Policy Q&A"]
        C3["Collect info (dates, type)"]
        C4["Extract from images"]
        C5["Recommend (top 3-5)"]
        C6["Nudge & remind"]
        C7["Route to right page"]
    end

    subgraph UI["UI does"]
        U1["Complex forms (final submit)"]
        U2["Data tables (50+ rows)"]
        U3["Calendar visual selection"]
        U4["Preview & confirm extracted data"]
        U5["Browse full catalogs"]
        U6["Dashboards & charts"]
        U7["MFA & sensitive operations"]
    end

    Chat -->|"Golden Rule:<br/>AI does the hard work<br/>(input, search, extract)"| UI
    UI -->|"Human does the satisfying work<br/>(verify, confirm, decide)"| Chat

    style Chat fill:#e3f2fd,stroke:#2196f3
    style UI fill:#fff3e0,stroke:#ff9800
```

---

## 5. Module Coverage

| Module | Milestone 1 (MVP) | Milestone 2 | Milestone 3 | Milestone 4 |
|--------|-------------|--------|--------|--------|
| **Profile** | — | Lookup (CV, Skills, Education, Certs) | — | Team Analytics |
| **Training** | — | My Trainings, TOEIC | — | — |
| **Attendance** | — | Overview (check-in/out, hours) | — | — |
| **Leave/WFH** | Balance, History, WFH, Leave Submit, Vision AI | — | — | Notifications |
| **Device** | — | Info Query + IT Support | — | Notifications |
| **Buddy** | — | My Buddy, My Buddees | — | — |
| **Tasks** | — | — | Task Manager (write) | — |
| **Company Regs** | HR Policy Q&A | All Regulations | — | Semantic Search |
| **External** | — | Slack Bot | — | — |
