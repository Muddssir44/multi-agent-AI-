⚙️ Multi-Agent AI Workflow with MCP Orchestration
An automated, resilient AI pipeline that processes incoming files, analyzes them with LLM agents, and performs smart follow-up actions. Built for extensibility and full transparency.

🚀 1. Project Overview
This project showcases a multi-agent AI architecture controlled via a custom Model Context Protocol (MCP) layer. The system ingests various file types (PDF, JSON, TXT), classifies them, delegates processing to dedicated agents, and routes actions based on analysis.

High-Level Flow:
Receive a file through an API.

Classify format and intent using a central agent.

Dispatch the file to a specialized agent (Email, PDF, or JSON).

Trigger downstream actions like CRM updates or alerts.

Log each step into a persistent, queryable memory layer using SQLite.

The SQLite memory store ensures crash recovery and enables live dashboards for tracking agent decisions and actions.

🧱 2. System Architecture


🧠 3. Workflow Breakdown
📤 File Upload Interface
Users upload files via a CLI or frontend (React).

🧩 FastAPI Backend (MCP Layer)
Generates a unique run_id.

Inserts metadata into SQLite.

Delegates to a classifier agent for file format and intent.

Sends the request to the matching agent for deeper processing.

🗃️ SQLite Memory (workflow_run Table)
Tracks all pipeline states.

Includes columns for classification, agent output, and action history.

Enables audit logging and traceability.

🔍 Classifier Agent
Uses LLM + rules to identify file format and purpose.

Outputs structured metadata used to drive routing.

📧 / 📄 / 🗃️ Specialist Agents
Email Agent: Extracts sender info, tone, urgency, and suggests resolution steps.

PDF Agent: Extracts structured content like invoice or compliance data.

JSON Agent: Validates structure, flags schema mismatches or anomalies.

⚡ Action Router
Reads suggested actions from memory.

Calls downstream services (e.g., CRM, alert system) with retry logic.

Logs outcome in memory store.

🔑 4. Core Features
Modular Agents: Custom logic per file type ensures specialized processing.

Crash-Safe Orchestration: MCP protocol uses persistent memory and retries.

Structured Memory Log: A single source of truth using SQLite with append-only logs.

Action Triggers: Automates downstream API calls based on agent decisions.

API First: Clean REST API with FastAPI + Swagger UI for testing and integration.

📁 5. Project Structure

multiagent/
├── .gitignore
├── LICENSE
├── README.md
├── app/                              # Backend (FastAPI server)
│   ├── __init__.py
│   ├── .env
│   ├── app.log
│   ├── config.py                     # Global configuration settings
│   ├── main.py                       # Primary FastAPI entry point
│   ├── main1.py                      # Alternate/main entry point (if applicable)
│   ├── memory.db                     # SQLite database for workflow history
│   ├── memory1.db                    # Alternative SQLite database copy
│   ├── agents/                       # Agent implementations (e.g., classification)
│   │   ├── __init__.py
│   │   └── ClassifierAgent.py
│   ├── core/                         # Core logic and utilities
│   │   ├── __init__.py
│   │   └── ActionRouter.py           # Routes actions based on agent results
│   ├── memory/                       # MemoryStore for database interaction
│   │   ├── __init__.py
│   │   └── MemoryStore.py
│   ├── processor/                    # File processing logic (extraction, OCR)
│   │   ├── __init__.py
│   │   └── fileProcessor.py
│   ├── router/                       # Routing logic for agents
│   │   ├── __init__.py
│   │   └── AgentRouter.py
│   └── test/                         # Testing scripts and notebooks
│       ├── __init__.py
│       └── testDB.ipynb
├── client/                           # Frontend (React/Vite)
│   ├── .gitignore
│   ├── eslint.config.js
│   ├── index.html
│   ├── package.json
│   ├── README.md
│   ├── vite.config.js
│   ├── public/
│   │   └── favicon.ico
│   └── src/
│       ├── App.jsx                   # Main React component handling UI and routing display
└── sampleFiles/                      # Sample documents for testing
    ├── Architecture.png              # Diagram of system architecture
    ├── json/
    │   └── sample1.json
    ├── pdfs/
    │   ├── invoice.pdf
    │   └── receipt.pdf
    └── txt/
        ├── sample1.txt
        └── sample3.txt


💾 6. Workflow Memory: workflow_run Table
A SQLite-powered ledger that tracks:

File metadata (type, source, format)

Classification results and intent

Each agent’s output

Final actions and their statuses

A complete event history (JSON-logged)

This design allows step-by-step auditing and recovery.

Sample Schema:

CREATE TABLE workflow_run (
  run_id TEXT PRIMARY KEY,
  source TEXT,
  file_path TEXT,
  ...
  action_status TEXT,
  history TEXT
);

🧪 7. Testing
Testing is handled using both scripts and Jupyter notebooks:

testDB.ipynb: Explores and verifies the database entries.

test_agents.py: Runs sample files through agents for verification.

Modular tests ensure that each agent and routing decision works independently.

🛠️ 8. Installation & Setup
Setup instructions (env setup, Docker, frontend deployment) will be added soon as development progresses.



