🏦 Agentic Compliance Orchestrator
A Multi-Agent Workflow for Customer Onboarding & Sanctions Screening
📌 Overview

This project demonstrates a lightweight multi-agent compliance workflow built using ADK. It automates the early stages of private banking client onboarding by collecting basic PII, running sanctions and adverse-media checks, and generating a structured compliance readiness report.

The system showcases multi-agent orchestration, custom + built-in tools, session-based memory, and tool-augmented decision-making, fulfilling the core learning objectives of the Kaggle x Google AI Agents course.

🧩 Problem Statement

Onboarding High-Net-Worth (HNI/UHNI) clients in private banking is a manual, slow, and regulation-heavy process. Relationship managers must verify client information, run sanctions and adverse-media checks, and compile compliance summaries before account creation.

This project solves the problem by automating the initial compliance triage, reducing onboarding friction and enabling faster decision-making.

🤖 Why Agents?

Agents provide capabilities that traditional backend services cannot:

Reason about workflow decisions (e.g., which tools to call)

Collaborate through multiple specialized agents

Persist and retrieve session state for multi-step processes

Combine custom compliance tools with built-in search tools

Easily extend the workflow with new tools or agents

Agents transform static KYC checks into an adaptive, intelligent compliance pipeline.

🏗️ What I Created — Architecture
High-Level Components

Web UI
Simple frontend form collecting:

First name

Last name

Date of birth

Account type

Python Backend (FastAPI)

Accepts PII

Generates session IDs

Stores data in SQLite

Invokes orchestrator & summary agents

Returns final compliance output

Session Database
SQLite store holding:

PII

Sanctions & media check results

Final verdict

Multi-Agent System (ADK)
Orchestrator Agent

loads session

decides which tools to call

performs sanctions & adverse media checks

updates memory

delegates to summary agent

Summary Agent

loads final state

generates compliance report + decision

Tools

watchman_sanctions_check() (custom tool)

Built-in search tool (simulated adversarial media lookup)


Workflow Diagram

    graph TD
    A[Start: User Accesses Web UI] --> B(HTTP POST: /onboard_customer);
        B --> C{1. Receive PII from UI};
        C --> D[2. Generate Session ID];
        D --> E[3. Save PII & Session ID to DB];
        style E fill:#aaffaa,stroke:#3c3;
    end

    subgraph ADK Agent System (app/agents/)
        E --> F{Onboarding Orchestrator Agent};
        F -- Retrieves Session from DB --> G[4. Orchestrator Loads Session State];
        style G fill:#aaffaa,stroke:#3c3;

        G --> H{Agent Determines Tool Needs};
        
        H --> I[5. Call Custom Tool: watchman_sanctions_check()];
        style I fill:#ADD8E6,stroke:#0077B6;
        
        H --> J[6. Call Built-in Tool: Google Search (Adverse Media)];
        style J fill:#FFD700,stroke:#FF8C00;

        I & J --> K[7. Update Session State in DB with Tool Results];
        style K fill:#aaffaa,stroke:#3c3;

        K --> L[8. Orchestrator Delegates to Summary Agent];
        L --> M[Summary Agent Loads Session from DB];
        M --> N(9. Generate Structured Compliance Report & Verdict);
    end

    N --> O{app/server.py: Render Result to UI};
    O --> P[End: Display Verdict to User];


🎥 Demo

Workflow:

User submits onboarding form

Backend stores session + triggers agent workflow

Orchestrator calls sanctions & media tools

Summary agent generates compliance decision

UI displays the results

(Add your screenshots or video link here.)

🛠️ The Build — Tools & Technologies Used
Frontend

HTML + JavaScript form

Backend

Python (FastAPI)

SQLite for lightweight state persistence

Agents

Orchestrator Agent

Summary Agent

Session-based memory using SQLite backend

Tools

Custom Tool: Watchman-based sanctions check

Built-in Tool: Google-like search for adverse media

ADK Features Used

Multi-agent workflow (sequential)

Custom & built-in tools

Session + memory management

Context-driven workflow logic

🚀 If I Had More Time…
1. Integrate real global sanctions APIs

Refinitiv, ComplyAdvantage, or OFAC endpoints.

2. Implement real Google Search Tool

Live adverse media signals instead of mock results.

3. Build dynamic forms

JSON-based templates for full KYC data.

4. Add more agents

Document verification

AML risk scoring

Enhanced identity validation

5. Observability

Logs, metrics, and tracing for workflow transparency.

6. Deploy to Cloud

Containerized deployment on GCP Cloud Run or App Engine.