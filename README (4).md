# 🚀 AI-Enabled CRM Application

A collaborative AI-Enabled Customer Relationship Management (CRM) application designed to manage contacts, leads, deals, tasks, activities, analytics, and AI-powered sales assistance in one platform.

---

## 🎯 Project Objective

The goal of the project was to build a modular CRM application where different functional areas could be developed independently and integrated into a unified system.

The application combines traditional CRM functionality with AI-powered insights and assistance to help users understand customer data and take informed actions.

---

## 👥 Team & Module Responsibilities

| Module | Responsibility | Team Member |
|--------|----------------|--------------|
| Module 1 | Authentication & User Management | Shamsu Nisha |
| Module 2 | Contacts & Lead Management | Agathiyan |
| Module 3 | Deals & Pipeline Management | Nishaj |
| Module 4 | Tasks, Activities & Notifications | Athim |
| Module 5 | Dashboard, Reporting & AI Integration | Ilavarasan |

---

## 🤖 My Contribution — Module 5

I was responsible for **Dashboard, Reporting & AI Integration**.

### 📊 Dashboard
- Dashboard metrics
- Pipeline funnel visualization
- Performance trends
- 7-day, 30-day and 90-day filtering
- Pipeline value and win-rate calculations

### 📑 Reporting
- Pipeline summary APIs
- Performance reporting
- CSV report export
- PDF/print report functionality

### 🧠 AI Features

**AI Lead Scoring**
- Score
- Category
- Reasons
- Recommendations

**AI Activity Summary**
- Activity summary
- Key points
- Follow-up suggestions

**Next Best Action**
- Recommended action
- Reason
- Priority

**AI Assistant**
- Contact information retrieval
- Deal information retrieval
- Activity retrieval
- Task retrieval
- Tool-use based CRM interaction

---

## 🔄 AI Provider Fallback

Implemented an LLM fallback architecture:

```
Groq
  ↓
OpenRouter
  ↓
Gemini
```

This allows the system to continue using another configured provider when a provider encounters retryable failures.

---

## 🔗 Module Integration

Module 5 integrates with the existing CRM modules:

```
Contacts ──┐
Deals ─────┤
Tasks ─────┼──→ Module 5 → Dashboard + Reports + AI
Activities ┘
```

The module uses the shared CRM data and authentication context rather than maintaining separate CRM data.

---

## 🏗️ Technology Stack

```
Frontend        → React, Vite
Backend         → Node.js, Express.js
Database        → PostgreSQL
API             → REST APIs
AI              → Groq, OpenRouter, Gemini
Visualization   → Recharts
Authentication  → JWT
Version Control → Git & GitHub
```

---

## 🧪 Testing & Validation

The implemented functionality was tested across:

- Dashboard metrics
- Date-range filtering
- Pipeline reporting
- CSV/PDF exports
- AI lead scoring
- Activity summaries
- Next-best-action
- AI assistant tools
- Deal creation and stage updates
- Deal deletion
- Data persistence after refresh
- Frontend production build

---

## 💡 Key Learning

This project gave me practical experience in:

- Full-stack application development
- REST API integration
- PostgreSQL
- React development
- LLM integration
- AI tool-use architecture
- Multi-provider AI fallback
- Debugging and integration
- Git-based team collaboration
- Working with a shared codebase

It was also my first team-based project, giving me hands-on experience in developing an individual module and integrating it with modules developed by other team members.

---

## 🔗 My Module 5 Implementation

**GitHub Branch:**
https://github.com/agathi0708/ai-enabled-crm/tree/feature/module-5-dashboard-ai

---

## 🙌 Acknowledgement

Thanks to my teammates for the collaboration and for making this a valuable hands-on development experience.

---

`#AI` `#CRM` `#GenerativeAI` `#LLM` `#React` `#NodeJS` `#PostgreSQL` `#FullStackDevelopment` `#SoftwareDevelopment` `#TeamProject`
