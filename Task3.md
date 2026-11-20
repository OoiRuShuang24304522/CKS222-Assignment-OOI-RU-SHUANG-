
# Task 3: Symmetrical Verification & Validation (V-Model Design)

**Case Study:** Grab Malaysia — Proposed *GrabCampus* Facility & Shuttle Booking with GrabPay Checkout  
**Scope:** Symmetrical V-Model V&V Architecture for the Atomic Slot-Lock & GrabPay Settlement Engine  
**Evidence Boundary:** *GrabCampus* is the group's proposed case-study feature integrating a USM facility booking workflow with a GrabPay payment flow. All architectural and testing controls below represent targeted process improvements to prevent late-stage defect leaks.

---

## 3.1 Custom V-Model Symmetrical Mapping: GrabCampus Atomic Slot-Lock & GrabPay Checkout Engine

To eliminate late-stage defect leaks and costly production rework, ad-hoc end-of-lifecycle manual testing is replaced with a **symmetrical V-Model Verification and Validation (V&V) architecture**. Because the cost of remediating defects escalates exponentially across later SDLC stages, the V-Model enforces preventive quality assurance by pairing every constructive design phase on the descending left arm with a horizontally aligned analytical testing phase on the ascending right arm.

During each left-side design activity, engineers simultaneously author the test specifications for its counterpart right-side testing phase, establishing 1-to-1 requirements traceability and integrating directly with Task 1's SPEM 2.0 review gates and Task 4's TDD/CI pipeline.

    CONSTRUCTIVE DESIGN PHASES (Left Arm)                         ANALYTICAL V&V PHASES (Right Arm)
    [Building the System Right]                                   [Testing Against Specifications & Needs]

    +---------------------------------------+                     +---------------------------------------+
    | 1. Business & User Requirements       |  =================> | 1. User Acceptance Testing (UAT)      |
    |    - INVEST User Stories & GWT (T2)   |   Parallel UAT Plan |    - Staging Court Booking & QR Flow  |
    |    - Product Requirements Baseline    |   (Validation)      |    - Security & UAT Sign-off Report   |
    +---------------------------------------+                     +---------------------------------------+
               \                                                                     /
                \                                                                   /
                 v                                                                 ^
       +------------------------------------+                         +------------------------------------+
       | 2. Software & System Requirements  |  =====================> | 2. System & Non-Functional Testing |
       |    - 3-Min Lock & Ledger Rules     |   Parallel System Test  |    - Peak Load & Latency (< 2.0s)  |
       |    - NFR SLAs & Biometric MFA Spec |   (Verification & Val.) |    - Failover & Security Pen-Test  |
       +------------------------------------+                         +------------------------------------+
                  \                                                               /
                   \                                                             /
                    v                                                           ^
          +---------------------------------+                       +---------------------------------+
          | 3. Microservice & API           |  ===================> | 3. Subsystem & API Integration  |
          |    Architectural Design         |   Parallel Int. Plan  |    Testing                      |
          |    - Microservice & API Blueprint|   (Verification)      |    - Contract & Rollback Tests  |
          |    - 3-Min Distributed Lock Flow|                       |    - Concurrent Slot-Lock Races   |
          +---------------------------------+                       +---------------------------------+
                     \                                                         /
                      \                                                       /
                       v                                                     ^
             +-------------------------------+                     +-------------------------------+
             | 4. Detailed Module & DB       |  =================> | 4. Automated Unit & Static    |
             |    Schema Design              |   Parallel TDD Spec |    Testing                    |
             |    - Atomic Lock & Wallet SQL |   (Verification)    |    - TDD Unit Suite (TDD-01..04)|
             |    - HMAC Payload Signing     |                     |    - SAST & SPEM Peer PR Gate   |
             +-------------------------------+                     +-------------------------------+
                        \                                                   /
                         \                                                 /
                          +----> +-----------------------------------+ ---+
                                 | 5. Implementation & Secure Coding |
                                 |    - Booking & GrabPay Source Code|
                                 |    - Defensive Coding & Refactor  |
                                 +-----------------------------------+

### Horizontal Phase-by-Phase Alignment

1. **Level 1: Business & User Requirements <-> User Acceptance Testing (UAT)**
   * **Constructive Phase (Left):** The **Product Manager (PM)** translates student and facility-admin needs into formal **INVEST User Stories** and **Given-When-Then (GWT)** acceptance criteria within the *Product Requirements & User Stories* baseline (Task 2), eliminating informal requirement creep. Concurrently, the PM and **QA Automation Engineer** draft the **UAT Plan** directly from the GWT positive and boundary scenarios.
   * **Analytical Phase (Right):** Before release, the QA Automation Engineer and PM execute business-level **User Acceptance Testing** in a production-mirrored staging environment—validating end-to-end RM 10.00 sports court booking, RM 20.00 wallet checkout, QR pass generation, and RM 9.99 insufficient-balance rejection—culminating in the *Security & UAT Sign-off Report* (Task 1).
   * **Symmetrical Link:** Every GWT scenario defined on the left maps 1-to-1 to an executable UAT test case on the right, validating that the completed feature satisfies real stakeholder needs.

2. **Level 2: Software & System Requirements (SRS) <-> System & Non-Functional Testing**
   * **Constructive Phase (Left):** The **Tech Lead / Architect** and PM formalize system-wide functional rules (three-minute temporary slot-lock lifecycle, idempotent GrabPay settlement states), non-functional Quality Attributes (`< 2.0s` end-to-end checkout response time), and external Technical Constraints (Local Biometric MFA for high-value settlements). Simultaneously, they author the **System & Performance Test Specification** defining quantitative pass/fail thresholds.
   * **Analytical Phase (Right):** The QA Automation Engineer and **DevOps Engineer** execute end-to-end **System Testing**, including peak-hour load testing, gateway timeout failover testing, and security vulnerability scanning across the integrated staging cluster.
   * **Symmetrical Link:** System test suites directly measure integrated runtime behavior against the quantitative NFR SLAs and security constraints locked in during SRS specification.

3. **Level 3: Microservice & API Architectural Design <-> Subsystem & API Integration Testing**
   * **Constructive Phase (Left):** The Tech Lead / Architect produces the *Microservice & API Architecture Blueprint* (Task 1), defining service contracts between the GrabCampus Booking Service, Slot-Lock Manager, GrabPay Wallet Ledger Service, and QR Pass Generator, alongside distributed concurrency-control mechanisms (atomic Redis/DB locks with automatic 3-minute TTL expiry). Parallel **API Contract & Concurrency Test Plans** are authored to define expected payloads and compensating rollback flows.
   * **Analytical Phase (Right):** The **Software Engineer (Backend Microservice Developer)** and QA Automation Engineer run **Subsystem & API Integration Testing** inside the *Automated Unit & Integration Test Suite* (Task 1), verifying API schema conformance, state rollbacks when GrabPay times out, and simultaneous booking attempts by two students for the same slot.
   * **Symmetrical Link:** Architectural interface contracts and distributed lock designs on the left are verified against real inter-service communication and database contention on the right prior to full system assembly.

4. **Level 4: Detailed Module & Database Schema Design <-> Automated Unit & Static Testing**
   * **Constructive Phase (Left):** The Software Engineer designs granular service classes, atomic database lock/deduction queries, and HMAC cryptographic signature routines. Following Task 4's Test-First (TDD) workflow, granular **Unit Test Cases** (`TDD-01` through `TDD-04`) are written concurrently with module design before implementation code is authored.
   * **Analytical Phase (Right):** During code construction and Task 1's SPEM 2.0 Pull Request workflow, GitHub Actions CI automatically executes the **TDD Unit Test Suite** alongside **Static Code Analysis (SAST)** to produce the *Automated Static Analysis & Security Scan Report*, followed by **Senior Peer Code Review** to produce the *Code Review Approval & Sign-off Log*.
   * **Symmetrical Link:** Low-level algorithmic branches, lock-expiry timers, and wallet balance checks specified on the left are verified at the code-unit level on the right, preventing logic defects from propagating into integration builds.

5. **Apex: Implementation & Secure Coding**
   * At the base of the 'V', the Software Engineer implements the *Booking & GrabPay Integration Source Code* (Task 1) using Pair Programming, defensive input validation, and continuous refactoring (Task 4) so every module is modular, free of code smells, and inherently verifiable.

### Summary Horizontal Traceability Matrix

| V-Model Level | Left-Side Constructive Phase & Artifact | Accountable / Responsible Role (Task 1 RACI) | Parallel Test Design Bridge | Right-Side Analytical Phase & Artifact | V&V Assurance Focus |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Level 1 (Top)** | **Business & User Requirements**<br>*(Product Requirements & User Stories)* | **A:** Product Manager<br>**C:** Tech Lead, QA | UAT Plan & GWT Acceptance Scenarios | **User Acceptance Testing (UAT)**<br>*(Security & UAT Sign-off Report)* | **Validation:** Confirms booking, payment, and QR pass workflows meet user expectations. |
| **Level 2** | **Software & System Requirements**<br>*(SRS, NFR SLAs & Constraint Specs)* | **A:** Tech Lead / Architect<br>**R:** Product Manager | System & Performance Test Specification | **System & Non-Functional Testing**<br>*(Load, Failover & Security Reports)* | **Verification & Validation:** Confirms end-to-end `< 2.0s` SLA and security constraint compliance. |
| **Level 3** | **Microservice & API Architecture**<br>*(Microservice & API Blueprint)* | **A:** Tech Lead / Architect<br>**R:** Software Engineer | API Contract & Concurrency Test Suite | **Subsystem & API Integration Testing**<br>*(Automated Integration Test Suite)* | **Verification:** Confirms service interfaces, failed-payment rollbacks, and 3-minute slot locks. |
| **Level 4** | **Detailed Module & DB Design**<br>*(Module Logic, Lock & Wallet Schemas)* | **A/R:** Software Engineer<br>**Reviewer:** Senior Peer Reviewer | TDD Unit Specs (`TDD-01`–`TDD-04`) & SAST Rules | **Automated Unit & Static Testing**<br>*(Static Scan Report & PR Sign-off Log)* | **Verification:** Confirms unit logic accuracy and zero critical static/security defects pre-merge. |

---

## 3.2 Quality Attribute & Technical Constraint Verification vs. Validation

In software quality engineering, **Verification** and **Validation** serve distinct, complementary roles prior to production deployment:
* **Verification (Activities — *"Did we build the system right?"*):** Consists of static inspections, architectural reviews, static code analysis, and controlled test executions performed by engineers to confirm that software artifacts strictly conform to written technical specifications and design rules.
* **Validation (Outcomes — *"Did we build the right system?"*):** Consists of empirical, operational outcomes demonstrating that the integrated system achieves its intended business Service Level Agreements (SLAs), user experience expectations, and real-world security guarantees under realistic operating conditions.

### 1. Non-Functional Quality Attribute (Chosen Internally): End-to-End GrabPay Checkout & Slot-Lock Response Time (`< 2.0 seconds`)

* **Verification (Engineering Activities):**
  1. **Architectural & Database Index Inspection:** The Tech Lead / Architect performs a static design review of the *Microservice & API Architecture Blueprint* and database execution plans (`EXPLAIN ANALYZE`) to verify that 3-minute slot-lock queries use composite indexes (avoiding full-table scans) and non-blocking asynchronous events for QR pass and receipt notifications.
  2. **Component Latency Profiling:** The Software Engineer executes automated API integration benchmarks in the GitHub Actions CI pipeline to verify that internal microservice execution, lock acquisition, and wallet ledger commit operations complete within an internal latency budget of `< 350 ms`, preserving headroom for external GrabPay gateway round-trips.
  3. **Automated Load & Stress Test Execution:** Using automated load-testing scripts (k6/JMeter), the QA Automation Engineer simulates peak campus demand (e.g., 5,000 concurrent student facility/shuttle booking requests per minute) against the staging cluster to measure response time distributions under lock contention.
* **Validation (Pre-Release & Canary Outcomes):**
  1. **Staging SLA Attainment Outcome:** Under sustained peak-load simulation in a production-mirrored staging environment, **at least 99% (p99) of completed booking-and-GrabPay checkout transactions achieve a total user-perceived end-to-end response time strictly below `2.0 seconds`**, with zero HTTP `504` timeouts or database deadlock errors.
  2. **Canary Telemetry Sign-Off Outcome:** During a controlled 5% canary rollout monitored by the DevOps Engineer, real-user Application Performance Monitoring (APM) dashboards confirm that live mobile client checkout and QR pass generation remain below `2.0 seconds` across 4G/5G network profiles without triggering duplicate client payment retries.

### 2. Technical Constraint (Mandated Externally): Local Biometric Multi-Factor Authentication (MFA) Integration

To comply with external mobile platform security models (iOS Face ID/Touch ID and Android BiometricPrompt via FIDO2/WebAuthn) and digital wallet governance, high-value GrabPay settlements or wallet top-ups (e.g., exceeding RM 250) require **Local Biometric MFA** before funds are debited.

* **Verification (Engineering Activities):**
  1. **Cryptographic Protocol & API Contract Inspection:** The Tech Lead / Architect and DevSecOps / QA Lead inspect the authentication sequence design to verify that raw biometric templates never leave the user's device Secure Enclave / Trusted Execution Environment (TEE), and that the backend verifies single-use cryptographic challenge nonces, device public-key signatures, and TLS 1.3 channel binding.
  2. **Static Application Security Testing (SAST) & SPEM Peer Review:** During Task 1's SPEM 2.0 Pull Request gate, automated SAST scanners and the Senior Peer Code Reviewer inspect the authentication module to verify that no developer backdoors, hardcoded bypass flags, or weak fallback tokens exist in the source code.
  3. **Negative & Boundary Security Test Execution:** The QA Automation Engineer executes automated security test cases covering expired challenge nonces, replayed biometric assertions, mismatched device signatures, user-canceled biometric prompts, and fallback PIN escalation flows.
* **Validation (Pre-Release & Operational Outcomes):**
  1. **Cross-Device Biometric UAT Outcome:** Across physical iOS and Android test devices in UAT, legitimate users authorize high-value GrabPay transactions seamlessly via native biometrics with **100% cryptographic signature verification accuracy**, while tampered payloads, unauthenticated API calls, and replayed assertions are deterministically rejected with `401/403` security responses.
  2. **Formal Security Governance Approval Outcome:** The DevSecOps / QA Lead issues formal sign-off in the *Security & UAT Sign-off Report*, confirming that no high-value GrabPay wallet deduction can execute without a verified hardware-backed biometric attestation, satisfying Grab's security governance and Malaysian e-payment regulatory constraints.
