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



sample input 


{
  "application_id": "APP-2025-001",
  "timestamp": "2024-12-02T10:30:00Z",
  "customer": {
    "personal_info": {
      "full_name": "John Michael Smith",
      "date_of_birth": "1975-03-15",
      "ssn": "XXX-XX-1234",
      "nationality": "USA",
      "us_person": true,
      "residential_address": {
        "street": "123 Main Street",
        "city": "New York",
        "state": "NY",
        "zip": "10001",
        "country": "USA"
      },
      "mailing_address": {
        "same_as_residential": true
      },
      "phone": "+1-212-555-0123",
      "email": "john.smith@email.com"
    },
    "financial_profile": {
      "net_worth": 15000000,
      "liquid_assets": 3500000,
      "annual_income": 850000,
      "source_of_wealth": "business_income",
      "source_of_wealth_details": "Real estate development business owned for 15 years",
      "expected_account_activity": {
        "initial_deposit": 2000000,
        "monthly_transactions": 50,
        "average_transaction_size": 75000,
        "expected_annual_volume": 5000000
      }
    },
    "employment": {
      "status": "self_employed",
      "occupation": "Real Estate Developer",
      "employer": "Smith Real Estate Development LLC",
      "years_in_current_role": 15,
      "industry": "real_estate"
    },
    "business_info": {
      "business_name": "Smith Real Estate Development LLC",
      "business_type": "LLC",
      "ein": "12-3456789",
      "date_established": "2009-03-01",
      "business_address": {
        "street": "456 Business Blvd",
        "city": "New York",
        "state": "NY",
        "zip": "10002",
        "country": "USA"
      },
      "nature_of_business": "Residential and commercial real estate development",
      "ownership_percentage": 100,
      "other_owners": []
    },
    "beneficial_owners": [
      {
        "name": "John Michael Smith",
        "dob": "1975-03-15",
        "ssn": "XXX-XX-1234",
        "ownership_percentage": 100,
        "control_person": true,
        "address": {
          "street": "123 Main Street",
          "city": "New York",
          "state": "NY",
          "zip": "10001"
        }
      }
    ],
    "pep_disclosure": {
      "is_pep": false,
      "is_family_of_pep": false,
      "is_close_associate_of_pep": false,
      "details": null
    },
    "tax_info": {
      "us_tax_resident": true,
      "tax_id_number": "XXX-XX-1234",
      "tax_country": "USA",
      "fatca_status": "US Person"
    }
  },
  "documents": [
    {
      "document_id": "DOC-001",
      "document_type": "government_id",
      "document_subtype": "drivers_license",
      "file_path": "/uploads/APP-2025-001/drivers_license.pdf",
      "file_name": "drivers_license.pdf",
      "file_size": 245678,
      "upload_date": "2024-12-02T09:15:00Z",
      "issuing_authority": "New York DMV",
      "document_number": "D123456789",
      "issue_date": "2020-03-15",
      "expiry_date": "2028-03-15"
    },
    {
      "document_id": "DOC-002",
      "document_type": "proof_of_address",
      "document_subtype": "utility_bill",
      "file_path": "/uploads/APP-2025-001/utility_bill.pdf",
      "file_name": "utility_bill.pdf",
      "file_size": 123456,
      "upload_date": "2024-12-02T09:16:00Z",
      "statement_date": "2024-11-15",
      "address_on_document": "123 Main Street, New York, NY 10001"
    },
    {
      "document_id": "DOC-003",
      "document_type": "financial_statement",
      "document_subtype": "bank_statement",
      "file_path": "/uploads/APP-2025-001/bank_statement.pdf",
      "file_name": "bank_statement_oct2024.pdf",
      "file_size": 567890,
      "upload_date": "2024-12-02T09:17:00Z",
      "statement_period": "2024-10-01 to 2024-10-31",
      "institution": "Chase Bank"
    },
    {
      "document_id": "DOC-004",
      "document_type": "business_document",
      "document_subtype": "articles_of_incorporation",
      "file_path": "/uploads/APP-2025-001/llc_formation.pdf",
      "file_name": "llc_formation_documents.pdf",
      "file_size": 345678,
      "upload_date": "2024-12-02T09:18:00Z",
      "state": "New York",
      "registration_number": "12345678"
    },
    {
      "document_id": "DOC-005",
      "document_type": "tax_document",
      "document_subtype": "tax_return",
      "file_path": "/uploads/APP-2025-001/tax_return_2023.pdf",
      "file_name": "form_1040_2023.pdf",
      "file_size": 456789,
      "upload_date": "2024-12-02T09:19:00Z",
      "tax_year": 2023
    }
  ],
  "account_details": {
    "account_type": "private_banking",
    "product": "premier_wealth_management",
    "requested_services": [
      "investment_advisory",
      "wealth_planning",
      "trust_services",
      "international_wire_transfers"
    ],
    "anticipated_countries": [
      "USA",
      "Canada",
      "UK"
    ]
  },
  "metadata": {
    "relationship_manager": "RM-12345",
    "branch": "New York Fifth Avenue",
    "referral_source": "existing_customer",
    "priority": "high",
    "sla_deadline": "2024-12-09T23:59:59Z"
  }
}