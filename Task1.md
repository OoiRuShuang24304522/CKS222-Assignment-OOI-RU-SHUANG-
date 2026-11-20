# Task 1: Process Definition & Modeling (Grab Malaysia)

**Notion Electronic Process Guide (EPG) Link:** https://app.notion.com/p/CKS222-Assignment-Ooi-Ru-Shuang-41a3730380e28052a264f1a9014ac87a?source=copy_link

---

## 1.1 SPEM 2.0 Process Model: Code Review & Static Analysis
To resolve ad-hoc merging and inconsistent code quality across Grab Malaysia's booking and payment microservices, the following standardized **Code Review & Static Analysis** workflow is modeled using SPEM 2.0 and implemented in Notion:

### A. Roles
* **QA & Acceptance Engineer:** Defines Definition of Ready (DoR) checks and Given-When-Then (GWT) acceptance criteria *before* implementation begins; maintains automated acceptance and integration test suites.
* **Backend Microservice Developer:** Implements reservation, concurrency locking, and payment workflows; executes GWT acceptance and unit tests locally during development to catch defects early.
* **Senior Peer Reviewer & DevSecOps:** Audits concurrency and race-condition logic, enforces automated static analysis and Definition of Done (DoD) quality gates, and signs off staging merges.

### B. Work Products 
| Work Product | Access Mode | Purpose & Quality Standard |
| :--- | :--- | :--- |
| **DoR-Verified Backlog Item & GWT Test Suite** | `Input`  | INVEST-compliant user story meeting DoR criteria, paired with pre-authored GWT acceptance scenarios covering happy and concurrency/edge paths. |
| **Feature Pull Request (PR) & Local Test Evidence** | `Output` <br>`Input` | GitHub PR containing feature and concurrency lock code plus local execution logs from GWT and unit test runs. |
| **Automated Gate Report & DoD Sign-Off Log** | `Output` | Verification record capturing CI/CD test execution, static analysis thresholds (test coverage and critical issue checks), peer reviews notes, and DoD staging approval. |

### C. Tasks & Sequential Steps
#### Task 1: Shift-Left GWT Specification & Local Feature Implementation
* **Performers:** `QA & Acceptance Engineer` & `Backend Microservice Developer` 
* **Input:** `DoR-Verified Backlog Item & GWT Test Suite`
* **Output:** `Feature Pull Request (PR) & Local Test Evidence`
* **Steps:**
  1. **Verify DoR Readiness:** `QA & Acceptance Engineer` confirms the backlog item satisfies INVEST and **Definition of Ready (DoR)** criteria before coding starts.
  2. **Author Pre-Code GWT Scenarios:** `QA & Acceptance Engineer` writes explicit **Given-When-Then (GWT)** acceptance criteria for happy and unhappy/concurrency paths prior to development.
  3. **Execute Shift-Left Local Tests:** `Backend Microservice Developer` implements the booking/locking logic and runs GWT and unit tests locally during development to detect defects and requirement gaps early.
  4. **Submit PR with Local Evidence:** `Backend Microservice Developer` commits the code, attaches local test logs to `Feature Pull Request (PR) & Local Test Evidence`, and opens a Pull Request for automated gating and review.

#### Task 2: Automated Quality Gating, Peer Audit & DoD Sign-Off
* **Performer:** `Senior Peer Reviewer & DevSecOps`
* **Input:** `Feature Pull Request (PR) & Local Test Evidence`
* **Output:** `Automated Gate Report & DoD Sign-Off Log`
* **Steps:**
  1. **Run Automated CI/CD & Static Checks:** `Senior Peer Reviewer & DevSecOps` oversees automated test suites and static analysis checks (e.g., coverage and vulnerability gates); any failure blocks merge progression.
  2. **Audit Concurrency & Business Logic:** `Senior Peer Reviewer & DevSecOps` inspects the PR for race conditions (such as double-booking or lock expiry edge cases) and code maintainability, requesting revisions if issues are found.
  3. **Verify Definition of Done (DoD):** `Senior Peer Reviewer & DevSecOps` verifies that all **DoD** criteria (automated test thresholds, zero critical issues, passing pipeline, resolved feedback) are met.
  4. **Record DoD Sign-Off & Merge:** `Senior Peer Reviewer & DevSecOps` records formal approval in `Automated Gate Report & DoD Sign-Off Log` and merges the PR into the staging branch.
---

## 1.2 RACI Governance Matrix (Grab Booking & Payment Lifecycle)
*Legend: **R** = Responsible, **A** = Accountable, **C** = Consulted, **I** = Informed*

| Lifecycle Deliverable | Product Manager (PM) | Tech Lead / Architect | Software Engineer | QA Automation Engineer | DevOps Engineer |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **1. Product Requirements & User Stories** | **A** | C | I | C | I |
| **2. Microservice & API Architecture Blueprint** | I | **A** | R | C | C |
| **3. Booking & GrabPay Integration Source Code** | I | C | **A / R** | I | I |
| **4. Automated Unit & Integration Test Suite** | I | C | R | **A** | I |
| **5. Security & UAT Sign-off Report** | C | C | I | **A / R** | C |
| **6. Production CI/CD Release & Monitoring** | I | C | I | C | **A / R** |
