# 🗓️ UniSched — Intelligent University Timetable ERP

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Flask-Backend-000000?style=for-the-badge&logo=flask&logoColor=white" alt="Flask">
  <img src="https://img.shields.io/badge/Supabase-PostgreSQL-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white" alt="Supabase">
  <img src="https://img.shields.io/badge/Google%20Gemini-AI%20Rule%20Builder-4285F4?style=for-the-badge" alt="Google Gemini">
  <img src="https://img.shields.io/badge/CSP-Backtracking%20Solver-8B5CF6?style=for-the-badge" alt="CSP">
  <img src="https://img.shields.io/badge/Vercel-Deployment-000000?style=for-the-badge&logo=vercel&logoColor=white" alt="Vercel">
</p>

<p align="center">
  <b>An intelligent timetable-generation ERP that combines constraint solving, local-search repair, AI-powered rule translation, validation, authentication, and multi-format scheduling exports.</b>
</p>

<p align="center">
  <a href="#-overview">Overview</a> •
  <a href="#-key-features">Features</a> •
  <a href="#-architecture">Architecture</a> •
  <a href="#-solver">Solver</a> •
  <a href="#-setup">Setup</a> •
  <a href="#-roadmap">Roadmap</a>
</p>

---

## 🧠 Overview

**UniSched** is a university timetable-generation ERP designed to automate the creation of weekly academic schedules under complex institutional constraints.

The system supports:

- 🏫 Multi-department scheduling
- 👨‍🏫 Faculty preferences
- 🧪 Shared laboratory resources
- 🧩 Structured scheduling constraints
- 🗣️ Natural-language scheduling rules
- ✅ Timetable validation
- 🔧 Automatic conflict repair
- 📤 Print / PDF / Excel / CSV exports
- 🔐 Authenticated ERP workflows

The core scheduling engine combines **Constraint Satisfaction Problem (CSP) backtracking** with **local-search repair heuristics** to search for feasible schedules and improve conflicting candidates.

The project also integrates **Google Gemini** to translate natural-language HOD scheduling rules into structured rule parameters.

---

# 🎯 Problem

University timetable generation becomes difficult when many constraints interact at the same time:

```text
Faculty Availability
        +
Class / Section Requirements
        +
Room & Laboratory Capacity
        +
Shared Resources
        +
Scheduling Rules
        +
Faculty Preferences
        +
Conflict Avoidance
        ↓
   Valid Timetable
```

UniSched brings those requirements into one scheduling workflow instead of relying entirely on manual timetable construction.

---

# 🚀 Key Features

## 🧩 Constraint-Based Timetable Solver

- Generates ranked timetable candidates
- Uses **backtracking CSP search**
- Evaluates soft-constraint penalties
- Searches for conflict-free or lower-penalty schedules

## 🔧 Local Search Repair

When candidate schedules contain occupancy conflicts, the repair engine attempts day/period swaps to improve feasibility.

```text
Candidate Schedule
        │
        ▼
Conflict Detection
        │
        ▼
Generate Repair Candidates
        │
        ▼
Day / Period Swaps
        │
        ▼
Re-evaluate Constraints
        │
        ▼
Improved Schedule
```

## 🤖 AI Rule Builder

HOD or administrator rules can be expressed in natural language.

```text
Natural-Language Rule
        │
        ▼
Google Gemini
        │
        ▼
Structured Parameters
        │
        ▼
Constraint Validation
        │
        ▼
Scheduler
```

This allows scheduling requirements to be translated into machine-readable rule objects before they reach the scheduling engine.

## ✅ Timetable Validation

The application includes a dedicated validation layer for checking timetable constraints and diagnosing schedule issues.

## 🔐 Authentication

The architecture includes authenticated ERP workflows and session-based access checks.

## 📤 Multi-Format Export

Generated schedules can be exported into:

- 🖨️ Print / PDF
- 📊 Styled Excel
- 📄 CSV
- 🌐 HTML-based print templates

---

# 🧠 Solver Architecture

```text
                ┌─────────────────────┐
                │   Scheduling Input  │
                └──────────┬──────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │ Candidate Generator │
                └──────────┬──────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │ CSP Backtracking    │
                │      Solver         │
                └──────────┬──────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │ Conflict Detection  │
                └──────────┬──────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │ Local Search Repair │
                │ Day / Period Swaps  │
                └──────────┬──────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │ Constraint         │
                │ Validation         │
                └──────────┬──────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │ Ranked Schedules    │
                └─────────────────────┘
```

---

# 🏗️ System Architecture

```text
┌─────────────────────────────────────────────────────────┐
│                  HTML / CSS / JavaScript                │
│                    ERP Frontend                         │
└──────────────────────────┬──────────────────────────────┘
                           │ REST API
                           ▼
┌─────────────────────────────────────────────────────────┐
│                    Flask API Layer                       │
├────────────┬────────────┬──────────────┬───────────────┤
│ Auth       │ Scheduling │ Validation   │ AI Rule Builder│
└─────┬──────┴──────┬─────┴──────┬───────┴──────┬────────┘
      │             │            │              │
      │             ▼            ▼              ▼
      │      ┌────────────┐ ┌───────────┐ ┌────────────┐
      │      │ CSP Solver │ │ Validator │ │   Gemini   │
      │      └─────┬──────┘ └───────────┘ └────────────┘
      │            │
      │            ▼
      │     Local Search Repair
      │
      ▼
┌─────────────────────────────────────────────────────────┐
│              Repository / Data Layer                    │
│         Supabase PostgreSQL / SQLite Fallback           │
└─────────────────────────────────────────────────────────┘
```

---

# 🧩 Project Modules

| Module | Responsibility |
|---|---|
| `app/core/` | Domain models and mapper utilities |
| `app/repository/` | Database access and repository implementations |
| `app/services/` | Local-search repair algorithms |
| `app/validators/` | Timetable validation and constraint checks |
| `app/exporters/` | Excel, CSV, HTML and print exports |
| `app/auth/` | Authentication and session verification |
| `app/ui/` | ERP frontend assets |
| `app/api/` | Flask API routes and server entry point |
| `app/ai/` | Gemini client, prompts and rule translation |
| `config/` | Environment/configuration loading |
| `database/` | SQLite files and PostgreSQL schema |
| `scripts/` | Migration and seed utilities |
| `tests/` | Automated test suites |

---

# 🤖 AI Rule Translation

The AI layer is designed to convert natural-language scheduling requirements into structured rule objects.

### Example Concept

```text
"Do not schedule Professor X on Friday afternoon"

                 ↓

        Gemini Rule Builder

                 ↓

{
  "faculty": "Professor X",
  "day": "Friday",
  "period": "afternoon",
  "constraint": "unavailable"
}

                 ↓

        Validation Layer

                 ↓

       Scheduling Engine
```

The structured representation can then be checked against the scheduling constraints before being applied.

---

# 🗄️ Database Architecture

UniSched supports two storage paths:

### 🟢 Supabase / PostgreSQL

Used when `DATABASE_URL` and the required Supabase configuration are provided.

### ⚪ SQLite Fallback

The application can fall back to SQLite when an external database URL is not configured.

This gives the project a lightweight local-development mode while preserving a path toward hosted database usage.

---

# 📁 Repository Structure

```text
TT_scheduler/
│
├── 📂 app/
│   ├── 📂 core/
│   ├── 📂 repository/
│   ├── 📂 services/
│   ├── 📂 validators/
│   ├── 📂 exporters/
│   ├── 📂 auth/
│   ├── 📂 ui/
│   ├── 📂 api/
│   └── 📂 ai/
│
├── 📂 config/
├── 📂 database/
├── 📂 docs/
├── 📂 scripts/
├── 📂 tests/
│   └── 📂 unit/
│
├── 🔐 .env.example
├── 🚫 .gitignore
└── 📘 README.md
```

---

# 🛠️ Tech Stack

<p align="center">

<img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python">
<img src="https://img.shields.io/badge/Flask-000000?style=flat-square&logo=flask&logoColor=white" alt="Flask">
<img src="https://img.shields.io/badge/Supabase-3ECF8E?style=flat-square&logo=supabase&logoColor=white" alt="Supabase">
<img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL">
<img src="https://img.shields.io/badge/Google%20Gemini-4285F4?style=flat-square" alt="Google Gemini">
<img src="https://img.shields.io/badge/SQLite-003B57?style=flat-square&logo=sqlite&logoColor=white" alt="SQLite">
<img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black" alt="JavaScript">
<img src="https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white" alt="HTML5">
<img src="https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white" alt="CSS3">
<img src="https://img.shields.io/badge/REST%20API-FF6F00?style=flat-square" alt="REST API">

</p>

---

# ⚙️ Setup

## ✅ Prerequisites

- Python 3.10+
- SQLite3 for local fallback, or a Supabase/PostgreSQL database
- Google Gemini API key for AI rule translation

## 1️⃣ Clone the Repository

```bash
git clone https://github.com/Divakar1326/TT_scheduler.git
cd TT_scheduler
```

## 2️⃣ Create a Virtual Environment

### Windows

```bash
python -m venv venv
venv\Scripts\activate
```

### macOS / Linux

```bash
python3 -m venv venv
source venv/bin/activate
```

## 3️⃣ Install Dependencies

```bash
pip install -r requirements.txt
```

## 4️⃣ Configure Environment Variables

Copy the example environment file:

```bash
cp .env.example .env
```

Configure the required values:

```env
SUPABASE_URL=
SUPABASE_KEY=
SUPABASE_SERVICE_ROLE_KEY=

DATABASE_URL=

GEMINI_API_KEY=

JWT_SECRET=

PORT=8000
```

> Never commit real secrets to the repository.

## 5️⃣ Initialize the Database

```bash
python scripts/migration.py
python scripts/seed_demo_data.py
```

For Supabase-backed setup:

```bash
python scripts/migration.py
python scripts/seed_supabase.py
```

---

# ▶️ Run Locally

Start the Flask application:

```bash
python -m app.api.app
```

Open:

```text
http://localhost:8000
```

---

# 🤖 Enable Gemini Rule Builder

Set:

```env
GEMINI_API_KEY=your_key_here
```

The AI Rule Builder can then be used to translate natural-language scheduling rules into structured scheduling parameters.

---

# 🧪 Testing

Run the test suite with your project's configured test command.

A typical Python test workflow is:

```bash
python -m pytest
```

The repository includes a dedicated `tests/` area for automated testing.

---

# 📊 Exporting Schedules

Generated schedules can be prepared for multiple output formats:

```text
Generated Schedule
       │
       ├──► 🖨️ Print / PDF
       ├──► 📊 Excel
       ├──► 📄 CSV
       └──► 🌐 HTML Print Template
```

---

# 🌐 Deployment

The repository includes deployment-oriented configuration and can be prepared for hosted deployment.

For a production deployment:

1. Configure environment variables.
2. Provision the PostgreSQL/Supabase database.
3. Configure the required API credentials.
4. Run migrations.
5. Build/deploy the application.
6. Validate authentication, scheduling, exports, and AI-rule workflows.

---

# 🔐 Security Notes

Because this system handles authentication, database credentials, and scheduling data:

- 🔑 Never commit secrets
- 🔐 Use strong production JWT/session secrets
- 🚫 Do not use example/default passwords in production
- 🗄️ Restrict database permissions appropriately
- ✅ Validate API inputs
- 🚦 Add rate limiting to exposed endpoints
- 🔒 Use HTTPS in production
- 🧪 Test authorization boundaries
- 📋 Review logs for accidental secret exposure

---

# 🗺️ Roadmap

### Planned Enhancements

- 🖱️ Drag-and-drop timetable editing
- 📧 Automated faculty email notifications
- 📱 SMS / notification support
- 🧰 Resource booking for projectors and auxiliary devices
- 📊 More advanced scheduling analytics
- 🧠 Expanded AI-assisted constraint generation
- 🔄 More sophisticated schedule optimization

---

# 🎯 Why This Project Stands Out

UniSched combines several software-engineering concepts that normally appear as separate projects:

```text
Constraint Programming
        +
Optimization Heuristics
        +
AI / LLM Integration
        +
REST APIs
        +
Authentication
        +
Database Systems
        +
Validation
        +
Data Export
        +
ERP-Style UI
        =
University Scheduling Platform
```

That combination makes the project a strong example of **applied algorithm engineering and full-stack software development**.

---

# 📄 License

This project is licensed under the **MIT License**.

---

# 👨‍💻 Author

## Divakar M

**B.Tech CSE — Artificial Intelligence & Data Science**

AI/ML • Generative AI • Python • Full-Stack Development

<p align="center">
  <a href="https://github.com/Divakar1326">
    <img src="https://img.shields.io/badge/GitHub-Divakar1326-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub">
  </a>
</p>

---

<p align="center">
  <b>Constraints → Intelligence → Timetables 🧠📅</b>
</p>
