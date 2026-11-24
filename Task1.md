# Task 1: Process Definition & Modeling (Shift-Left Quality Engineering Applied to Facility Booking & Concurrency Locking)

**Notion Electronic Process Guide (EPG) Link:** https://app.notion.com/p/CKS222-Assignment-Ooi-Ru-Shuang-41a3730380e28052a264f1a9014ac87a?source=copy_link

---

## 1.1 SPEM 2.0 Process Model: Code Review & Static Analysis
To resolve ad-hoc merging and inconsistent code quality across Grab Malaysia's booking and payment microservices, the following standardized **Code Review & Static Analysis** workflow is modeled using SPEM 2.0 and implemented in Notion:

### A. Roles
* **`QA & Acceptance Engineer`:** Defines Definition of Ready (DoR) checks and Given-When-Then (GWT) acceptance criteria *before* implementation begins; maintains automated acceptance and integration test suites.
* **`Backend Microservice Developer`:** Implements reservation, concurrency locking, and payment workflows; executes GWT acceptance and unit tests locally during development to catch defects early.
* **`Senior Peer Reviewer & DevSecOps`:** Audits concurrency and race-condition logic, enforces automated static analysis and Definition of Done (DoD) quality gates, and signs off staging merges.

### B. Work Products 
| # | Work Product | Access Mode | Purpose & Quality Standard |
| :--- | :--- | :--- | :--- |
| **WP1** | **DoR-Verified Backlog Item & GWT Test Suite** | `Input`  | INVEST-compliant user story meeting DoR criteria, paired with pre-authored GWT acceptance scenarios covering happy and concurrency/edge paths. |
| **WP2** | **Feature Pull Request (PR) & Local Test Evidence** | `Output` <br>`Input` | GitHub PR containing feature and concurrency lock code plus local execution logs from GWT and unit test runs. |
| **WP3** | **Automated Gate Report & DoD Sign-Off Log** | `Output` | Verification record capturing CI/CD test execution, static analysis thresholds (test coverage and critical issue checks), peer reviews notes, and DoD staging approval. |

### C. Tasks & Sequential Steps
#### Task 1: Shift-Left GWT Specification & Local Feature Implementation
* **Performers:** `QA & Acceptance Engineer & Backend Microservice Developer` 
* **Input:** `WP1: DoR-Verified Backlog Item & GWT Test Suite`
* **Output:** `WP2: Feature Pull Request (PR) & Local Test Evidence`
* **Steps:**
  1. **Verify DoR Readiness:** `QA & Acceptance Engineer` confirms the backlog item satisfies INVEST and **Definition of Ready (DoR)** criteria before coding starts.
  2. **Author Pre-Code GWT Scenarios:** `QA & Acceptance Engineer` writes explicit **Given-When-Then (GWT)** acceptance criteria for happy and unhappy/concurrency paths prior to development.
  3. **Execute Shift-Left Local Tests:** `Backend Microservice Developer` implements the booking/locking logic and runs GWT and unit tests locally during development to detect defects and requirement gaps early.
  4. **Submit PR with Local Evidence:** `Backend Microservice Developer` commits the code, attaches local test logs to `WP2: Feature Pull Request (PR) & Local Test Evidence`, and opens a Pull Request for automated gating and review.

#### Task 2: Automated Quality Gating, Peer Audit & DoD Sign-Off
* **Performer:** `Senior Peer Reviewer & DevSecOps`
* **Input:** `WP2: Feature Pull Request (PR) & Local Test Evidence`
* **Output:** `WP3: Automated Gate Report & DoD Sign-Off Log`
* **Steps:**
  1. **Run Automated CI/CD & Static Checks:** `Senior Peer Reviewer & DevSecOps` oversees automated test suites and static analysis checks; any failure blocks merge progression.
  2. **Audit Concurrency & Business Logic:** `Senior Peer Reviewer & DevSecOps` inspects the PR for race conditions (like double-booking or lock expiry edge cases) and code maintainability, requesting revisions if issues are found.
  3. **Verify Definition of Done (DoD):** `Senior Peer Reviewer & DevSecOps` verifies that all **DoD** criteria (automated test thresholds, zero critical issues, passing pipeline, resolved feedback) are met.
  4. **Record DoD Sign-Off & Merge:** `Senior Peer Reviewer & DevSecOps` records formal approval in `WP3: Automated Gate Report & DoD Sign-Off Log` and merges the PR into the staging branch.
---

## 1.2 RACI Governance Matrix (Grab Booking & Payment Lifecycle)
To enforce unambiguous accountability across 5-member reorganized team structure, the table below maps **six key deliverables** derived directly from 3 work products and 2 tasks across **five specialized roles**:  
* **`QA & Acceptance Engineer`** writes pre-code GWT acceptance criteria and checks DoR readiness, 
* **`Backend Microservice Developer`** builds booking/locking logic and runs local tests before opening PRs, 
* **`Senior Peer Reviewer & DevSecOps`** reviews concurrency logic, enforces automated quality gates, and approves staging merges, 
* **`Product Owner / Tech Lead`** owns backlog scope, DoR approval, and release alignment,
* **`UI / Frontend Developer`** handles client-side booking UI and frontend test alignment.

*Legend: **R** = Responsible, **A** = Accountable, **C** = Consulted, **I** = Informed*

| # | Lifecycle Deliverable | QA & Acceptance Engineer | Backend Microservice Developer | Senior Peer Reviewer & DevSecOps | Product Owner / Tech Lead | UI / Frontend Developer |
| :- | :--- | :---: | :---: | :---: | :---: | :---: |
| **D1** | **WP1: DoR-Verified Backlog Item & GWT Test Suite**| **R** | **C** | **I** | **A** | **C** |
| **D2** | **Concurrency Booking Core & Shift-Left Local Test Run**| **C** | **R / A** | **I** | **I** | **C** |
| **D3** | **WP2: Feature Pull Request (PR) & Local Test Evidence**| **I** | **R / A** | **I** | **I** | **I** |
| **D4** | **Automated CI/CD & Static Analysis Gate**| **C** | **C** | **R / A** | **I** | **C** |
| **D5** | **Concurrency Logic Audit & DoD Verification**| **I** | **C** | **R / A** | **I** | **I** |
| **D6** | **WP3: Automated Gate Report & DoD Sign-Off Log**| **I** | **I** | **R / A** | **C** | **I** |
