# AI-Enabled CRM — Module 5
## Dashboard, Reporting & AI Integration

Module 5 is responsible for the **Dashboard, Reporting, and AI Integration** capabilities of the AI-Enabled CRM system.

The module provides CRM performance visualization, report generation, AI-powered lead intelligence, AI sales recommendations, and a CRM conversational assistant.

---

## Table of Contents

1. [Module Overview](#1-module-overview)
2. [Features](#2-features)
3. [Reporting](#3-reporting)
4. [Report Export](#4-report-export)
5. [AI Integration](#5-ai-integration)
6. [AI Lead Scoring](#6-ai-lead-scoring)
7. [AI Contact Summary](#7-ai-contact-summary)
8. [AI Next Best Action](#8-ai-next-best-action)
9. [CRM Chat Assistant](#9-crm-chat-assistant)
10. [AI Tool Usage](#10-ai-tool-usage)
11. [Backend Architecture](#11-backend-architecture)
12. [Frontend Architecture](#12-frontend-architecture)
13. [API Endpoints](#13-api-endpoints)
14. [API Response Structure](#14-api-response-structure)
15. [Environment Configuration](#15-environment-configuration)
16. [Installation](#16-installation)
17. [Testing](#17-testing)
18. [Frontend Production Build](#18-frontend-production-build)
19. [Standalone Development Data](#19-standalone-development-data)
20. [Team Integration](#20-team-integration)
21. [Integration Architecture](#21-integration-architecture)
22. [Security Considerations](#22-security-considerations)
23. [Current Standalone Limitations](#23-current-standalone-limitations)
24. [Module 5 Deliverables](#24-module-5-deliverables)
25. [Development Status](#25-development-status)
26. [Module Ownership](#26-module-ownership)

---

## 1. Module Overview

### Module

**Module 5 — Dashboard, Reporting & AI Integration**

### Primary Responsibilities

Module 5 is responsible for:

- CRM dashboard metrics
- Pipeline visualization
- Performance trend analysis
- Report export
- AI lead scoring
- AI-generated contact summaries
- AI-powered next-best-action recommendations
- CRM conversational assistant
- AI tool usage for CRM data queries
- Multi-provider AI fallback

### Module Dependencies

Module 5 is designed to consume data from:

- **Module 2 — Contacts & Lead Management**
- **Module 3 — Deals & Pipeline Management**
- **Module 4 — Tasks, Activities & Notifications**

During standalone development, mock repositories are used so that Module 5 can be developed and tested independently.

During final team integration, these repositories will be connected to the team's actual CRM data sources.

---

## 2. Features

### 2.1 Dashboard

The dashboard provides an overview of CRM performance.

#### Dashboard Metrics

The dashboard currently displays:

- Total Leads
- Total Deals
- Open Deals
- Pipeline Value
- Win Rate

#### Pipeline Funnel

The pipeline funnel visualizes deal distribution across:

- Qualified
- Proposal
- Negotiation
- Won

Lost deals are maintained in the backend report data but are not displayed as an active pipeline stage in the funnel.

#### Performance Trend

The performance chart displays:

- Leads
- Won Deals

across monthly performance data.

---

## 3. Reporting

Module 5 provides reporting APIs and frontend report export functionality.

### Supported Reports

#### Pipeline Summary

Provides:

- Total leads
- Total deals
- Open deals
- Won deals
- Lost deals
- Win rate
- Total pipeline value
- Deal-stage counts
- Deal-stage values

#### Performance Report

Provides:

- Reporting range
- Monthly lead counts
- Monthly deal counts
- Won deal counts
- Revenue

Supported ranges:

- `7d`
- `30d`
- `90d`

---

## 4. Report Export

The dashboard supports exporting performance reports in two formats.

### CSV Export

Users can export the performance report as:

```
crm-performance-report.csv
```

### PDF Export

Users can export the performance report as:

```
crm-performance-report.pdf
```

PDF generation is handled on the frontend using **jsPDF**.

---

## 5. AI Integration

Module 5 integrates multiple AI providers through a common provider abstraction.

### AI Provider Order

The configured fallback order is:

```
Groq
  ↓
OpenRouter
  ↓
Gemini
```

The system attempts to use the primary provider first.

If the provider encounters a retryable failure such as:

- Rate limiting
- Server-side errors
- Timeout
- Connection failure

the system attempts the next configured provider.

---

## 6. AI Lead Scoring

The AI lead scoring service analyzes CRM contact information and produces a score between:

```
0 – 100
```

The response contains:

```json
{
  "contactId": 1,
  "score": 82,
  "rationale": "..."
}
```

### Lead Scoring Inputs

The current standalone implementation considers information such as:

- Contact status
- Number of interactions
- Recency of contact
- Potential deal value
- Contact/company information

### Output

The AI produces:

- Numerical lead score
- Explanation/rationale
- Provider used

---

## 7. AI Contact Summary

The contact summary service generates a concise summary based on:

- Contact information
- Recent CRM activities
- Interaction history

Example response structure:

```json
{
  "contactId": 1,
  "summary": "AI-generated contact summary...",
  "provider": "groq"
}
```

---

## 8. AI Next Best Action

The Next Best Action service analyzes:

- Contact status
- Recent activities
- Interaction frequency
- Recency of communication
- Pending tasks
- Potential deal value

It generates one recommended sales action.

Example:

```json
{
  "contactId": 1,
  "action": "Call Arun Kumar to follow up on the CRM proposal",
  "reason": "The proposal follow-up task is pending and due today...",
  "provider": "groq"
}
```

The service is designed to return a single practical action rather than a list of unrelated recommendations.

---

## 9. CRM Chat Assistant

Module 5 includes a conversational CRM assistant.

Users can ask questions about CRM information such as:

- Contacts
- Activities
- Deals
- Tasks

Example:

```
What activities does Arun have?
```

The assistant determines whether a CRM tool is required.

---

## 10. AI Tool Usage

The assistant uses a controlled tool registry rather than allowing the AI model to directly access the database.

Available tools include:

- `getContact`
- `getActivities`
- `getDeals`
- `getTasks`

The general flow is:

```
User Question
      ↓
Assistant
      ↓
Determine Required Tool
      ↓
Tool Registry
      ↓
CRM Data
      ↓
AI Response
      ↓
User
```

The assistant is designed to use only the information returned by the CRM tools and should not fabricate CRM data.

---

## 11. Backend Architecture

The standalone Module 5 backend follows a layered structure.

```
backend/
│
├── src/
│   │
│   ├── app.js
│   ├── server.js
│   │
│   ├── config/
│   │
│   ├── integrations/
│   │   └── ai/
│   │       ├── aiClient.js
│   │       └── aiProvider.js
│   │
│   ├── modules/
│   │   │
│   │   ├── reports/
│   │   │   ├── reports.controller.js
│   │   │   ├── reports.repository.js
│   │   │   ├── reports.routes.js
│   │   │   ├── reports.schema.js
│   │   │   └── reports.service.js
│   │   │
│   │   └── ai/
│   │       ├── ai.controller.js
│   │       ├── ai.repository.js
│   │       ├── ai.routes.js
│   │       ├── ai.schema.js
│   │       ├── ai.service.js
│   │       │
│   │       ├── prompts/
│   │       │   ├── assistant.prompt.js
│   │       │   ├── leadScoring.prompt.js
│   │       │   ├── nextBestAction.prompt.js
│   │       │   └── summary.prompt.js
│   │       │
│   │       ├── services/
│   │       │   ├── aiResponseParser.js
│   │       │   ├── assistant.service.js
│   │       │   ├── leadScoring.service.js
│   │       │   ├── nextBestAction.service.js
│   │       │   └── summary.service.js
│   │       │
│   │       └── tools/
│   │           ├── getActivities.js
│   │           ├── getContact.js
│   │           ├── getDeals.js
│   │           ├── getTasks.js
│   │           └── toolRegistry.js
│   │
│   └── tests/
│
├── package.json
└── package-lock.json
```

---

## 12. Frontend Architecture

The frontend is built using **React** and **Vite**.

```
frontend/
│
├── src/
│   │
│   ├── api/
│   │   ├── axiosClient.js
│   │   └── endpoints.js
│   │
│   ├── features/
│   │   │
│   │   ├── dashboard/
│   │   │   ├── api/
│   │   │   │   └── dashboardApi.js
│   │   │   ├── components/
│   │   │   │   ├── Dashboard.jsx
│   │   │   │   ├── DashboardFilters.jsx
│   │   │   │   ├── MetricCard.jsx
│   │   │   │   ├── PerformanceChart.jsx
│   │   │   │   ├── PipelineFunnel.jsx
│   │   │   │   └── ReportExport.jsx
│   │   │   ├── data/
│   │   │   │   └── mockData.js
│   │   │   └── hooks/
│   │   │       └── useDashboard.js
│   │   │
│   │   └── ai-assistant/
│   │       ├── api/
│   │       │   └── aiApi.js
│   │       ├── components/
│   │       │   ├── AIAssistantPanel.jsx
│   │       │   ├── ChatInput.jsx
│   │       │   ├── ChatMessage.jsx
│   │       │   ├── InsightCard.jsx
│   │       │   └── LeadScoreCard.jsx
│   │       └── hooks/
│   │           └── useAIAssistant.js
│   │
│   ├── App.jsx
│   ├── App.css
│   ├── index.css
│   └── main.jsx
│
└── package.json
```

---

## 13. API Endpoints

### Health

```
GET /api/v1/health
```

### Reports

**Pipeline Summary**

```
GET /api/v1/reports/pipeline-summary
```

**Performance**

```
GET /api/v1/reports/performance?range=30d
```

Supported values: `7d`, `30d`, `90d`

### AI

**Lead Score**

```
POST /api/v1/ai/lead-score/:contactId
```

**Contact Summary**

```
POST /api/v1/ai/summary/:contactId
```

**CRM Assistant**

```
POST /api/v1/ai/assistant/query
```

Request:

```json
{
  "query": "What activities does Arun have?"
}
```

**Next Best Action**

```
POST /api/v1/ai/next-best-action/:contactId
```

---

## 14. API Response Structure

The Module 5 standalone backend follows a simple success/data response structure.

Example:

```json
{
  "success": true,
  "data": {
    "contactId": 1,
    "score": 82,
    "rationale": "..."
  }
}
```

Errors are returned with an unsuccessful response and an explanatory message.

---

## 15. Environment Configuration

Create a local backend `.env` file.

> **Do not commit the actual `.env` file.**
> Use `.env.example` as the configuration reference.

Required AI configuration:

```env
PORT=5000

AI_PRIMARY_PROVIDER=groq
AI_PRIMARY_BASE_URL=https://api.groq.com/openai/v1
AI_PRIMARY_API_KEY=YOUR_GROQ_API_KEY
AI_PRIMARY_MODEL=openai/gpt-oss-120b

AI_SECONDARY_PROVIDER=openrouter
AI_SECONDARY_BASE_URL=https://openrouter.ai/api/v1
AI_SECONDARY_API_KEY=YOUR_OPENROUTER_API_KEY
AI_SECONDARY_MODEL=openrouter/free

AI_TERTIARY_PROVIDER=gemini
AI_TERTIARY_BASE_URL=https://generativelanguage.googleapis.com/v1beta
AI_TERTIARY_API_KEY=YOUR_GEMINI_API_KEY
AI_TERTIARY_MODEL=gemini-3.6-flash
```

**Never commit real API keys.**

---

## 16. Installation

### Backend

```bash
cd backend
npm install
```

Start development server:

```bash
npm run dev
```

Backend: `http://localhost:5000`

### Frontend

Open another terminal:

```bash
cd frontend
npm install
```

Start Vite:

```bash
npm run dev
```

Frontend: `http://localhost:5173`

---

## 17. Testing

**Backend Health Check**

```powershell
Invoke-RestMethod http://localhost:5000/api/v1/health
```

**Pipeline Summary**

```powershell
Invoke-RestMethod http://localhost:5000/api/v1/reports/pipeline-summary
```

**Performance Report**

```powershell
Invoke-RestMethod "http://localhost:5000/api/v1/reports/performance?range=30d"
```

**Next Best Action**

```powershell
$body = @{ contactId = 1 } | ConvertTo-Json

Invoke-RestMethod `
    -Method POST `
    -Uri http://localhost:5000/api/v1/ai/next-best-action/1 `
    -ContentType "application/json" `
    -Body $body
```

**CRM Assistant**

```powershell
$body = @{
    query = "What activities does Arun have?"
} | ConvertTo-Json

Invoke-RestMethod `
    -Method POST `
    -Uri http://localhost:5000/api/v1/ai/assistant/query `
    -ContentType "application/json" `
    -Body $body
```

---

## 18. Frontend Production Build

Run:

```bash
npm run build
```

The Vite production build should complete successfully.

> A large JavaScript chunk warning may appear because of frontend dependencies such as charting and PDF-generation libraries. This is a build warning rather than a build failure.

---

## 19. Standalone Development Data

The standalone Module 5 implementation currently uses mock CRM data for independent development and testing.

The mock data is used for:

- Contacts
- Activities
- Deals
- Tasks
- Dashboard performance data

This allows Module 5 to be developed without requiring all other modules to be operational.

---

## 20. Team Integration

During final integration, the standalone repositories will be replaced or connected to the actual team modules.

**Contacts** — Module 5 will consume contact/lead information from **Module 2 — Contacts & Lead Management**.

**Deals** — Dashboard pipeline reporting will consume deal information from **Module 3 — Deals & Pipeline Management**.

**Activities and Tasks** — AI summaries and next-best-action logic will consume **Module 4 — Tasks, Activities & Notifications**.

**Database** — The integrated version will use the team's existing PostgreSQL database and database access layer. The standalone mock repositories are therefore temporary integration adapters.

---

## 21. Integration Architecture

The intended integrated architecture is:

```
                    AI-Enabled CRM
                          │
          ┌───────────────┼───────────────┐
          │               │               │
      Contacts          Deals       Activities/Tasks
      Module 2          Module 3          Module 4
          │               │               │
          └───────────────┼───────────────┘
                          │
                          ▼
                    Module 5
             Dashboard & Reporting
                          │
                          ▼
                    AI Services
                          │
          ┌───────────────┼───────────────┐
          │               │               │
      Lead Score      Summaries       Next Action
          │               │               │
          └───────────────┼───────────────┘
                          │
                    CRM Assistant
                          │
                    Tool Registry
                          │
          ┌───────────────┼───────────────┐
          ▼               ▼               ▼
       Contacts         Deals          Activities
```

---

## 22. Security Considerations

The module follows these principles:

- API keys are stored in environment variables.
- Real API keys must not be committed to Git.
- AI tools are restricted to predefined CRM operations.
- The assistant does not receive unrestricted database access.
- Raw SQL is not generated by the conversational assistant.
- CRM responses should be based on available CRM data.
- Provider credentials are not exposed to the frontend.

---

## 23. Current Standalone Limitations

The standalone version intentionally has some limitations.

**Mock CRM Data** — The report and AI repositories currently use mock data.

**Reporting Ranges** — The standalone performance dataset is fixed mock data. The `7d`, `30d`, and `90d` parameters are validated, but the mock dataset is not yet dynamically filtered against real database timestamps.

**Authentication** — The standalone Module 5 environment does not represent the team's final authentication integration.

**Database Integration** — The final implementation will connect Module 5 to the team's PostgreSQL database and existing module repositories.

---

## 24. Module 5 Deliverables

The completed standalone implementation provides:

**Dashboard**
- [x] Dashboard metrics
- [x] Pipeline funnel
- [x] Performance trend chart
- [x] Dashboard range filter

**Reporting**
- [x] Pipeline summary API
- [x] Performance report API
- [x] CSV export
- [x] PDF export

**AI**
- [x] AI lead scoring
- [x] Contact summary
- [x] Next-best-action
- [x] CRM chat assistant
- [x] Controlled CRM tools
- [x] Multi-provider fallback

**AI Providers**
- [x] Groq
- [x] OpenRouter
- [x] Gemini
- [x] Provider fallback handling

**Validation**
- [x] Backend health check
- [x] Report API testing
- [x] AI API testing
- [x] Frontend production build
- [x] CSV export validation
- [x] PDF export validation
- [x] CRM assistant validation

---

## 25. Development Status

**Standalone Module 5:** Complete
**Team Integration:** Pending

The current branch contains the standalone implementation for Module 5. Final integration will connect the dashboard and AI services to the team's existing Contacts, Deals, Activities, Tasks, Authentication, and PostgreSQL components.

---

## 26. Module Ownership

**Module:** Module 5 — Dashboard, Reporting & AI Integration

**Primary Responsibilities:**
- Dashboard
- Reporting
- AI lead intelligence
- AI assistant
- AI sales recommendations
- AI provider integration

**Dependencies:**
- Module 2 — Contacts & Lead Management
- Module 3 — Deals & Pipeline Management
- Module 4 — Tasks, Activities & Notifications

**Project Repository — AI-Enabled CRM:**
https://github.com/agathi0708/ai-enabled-crm

**Module 5 development branch:**
`feature/module-5-dashboard-ai`
