# Task 3: Symmetrical Verification & Validation (V-Model Design)

To eliminate late-stage defect leaks in Grab Malaysia’s **GrabCampus Integrated Booking and GrabPay Checkout** engine, ad-hoc end-of-lifecycle manual testing is replaced with a **symmetrical V-Model Verification and Validation (V&V) architecture**. In alignment with our hybrid tailoring strategy (Task 5), consumer UI and promotional updates run in 2-week Scrum sprints, whereas strict V-Model gates govern high-risk backend transaction, concurrency, and security modules—automated inside our GitHub Actions CI/CD pipeline so sprint velocity is preserved. Because defect remediation costs escalate exponentially across later SDLC stages, every constructive design activity on the descending left arm is horizontally paired with an analytical testing phase on the ascending right arm.

---

## 3.1 Custom V-Model Symmetrical Mapping: Atomic Slot-Lock & GrabPay Checkout Engine


*Figure 3.1: Custom Symmetrical V-Model mapping left-side constructive design phases to right-side analytical V&V phases for GrabCampus.*

### Mechanics of Horizontal Symmetry & Parallel Test Engineering

Rather than treating testing as a post-coding afterthought, engineers author right-side test specifications **in parallel** with left-side design artifacts. When user stories, system rules, microservice APIs, or database lock schemas are drafted on the left arm, their corresponding acceptance, system, integration, and unit test plans are created concurrently. This 1-to-1 horizontal bridge enforces full requirements traceability, catches specification ambiguities before coding begins, and executes automated quality gates through Task 1's SPEM 2.0 Pull Request workflow and Task 4's TDD/CI pipeline.

### Horizontal Traceability Matrix

| V-Model Level | Left-Side Constructive Phase & Artifact | Primary Roles (Task 1 & Task 5 Sync) | Parallel Test Bridge (Authored Concurrently) | Right-Side Analytical Phase & Artifact | V&V Assurance Focus |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Level 1 (Top)** | **Business & User Requirements**<br>*(INVEST User Stories & GWT Baseline)* | **Owner:** Product Manager<br>**Consulted:** Tech Lead, QA Automation Engineer | UAT Plan & GWT Acceptance Scenarios | **User Acceptance Testing (UAT)**<br>*(Security & UAT Sign-off Report)* | **Validation:** Confirms court/shuttle booking, GrabPay checkout, and edge-balance rejections (e.g., RM 9.99 vs. RM 10.00 fee) meet user needs. |
| **Level 2** | **Software & System Requirements**<br>*(SRS, NFR SLAs & MFA Constraint Specs)* | **Owner:** Tech Lead<br>**Consulted:** DevSecOps / QA Lead, DevOps Lead | System & Performance Test Specification | **System & Non-Functional Testing**<br>*(Load, Failover & Security Scan Reports)* | **Verification & Validation:** Confirms `< 2.0s` checkout response SLA, gateway timeout failover, and biometric MFA compliance. |
| **Level 3** | **Microservice & API Architecture**<br>*(Microservice & API Blueprint)* | **Owner:** Tech Lead<br>**Executor:** Backend Microservice Developer | API Contract & Concurrency Test Plan | **Subsystem & API Integration Testing**<br>*(Automated Integration Test Suite)* | **Verification:** Confirms service contracts, 3-minute slot state transitions (`Available` -> `Locked` -> `Booked`), and failed-payment rollbacks. |
| **Level 4** | **Detailed Module & DB Design**<br>*(Atomic Lock Queries & HMAC Specs)* | **Executor:** Backend Microservice Developer<br>**Reviewer:** Senior Peer Code Reviewer | TDD Unit Specs & SAST Ruleset | **Automated Unit & Static Testing**<br>*(Static Scan Report & PR Sign-off Log)* | **Verification:** Confirms unit-level branch logic, lock TTL expiry, and zero critical SAST flaws prior to branch merge. |
| **Apex (Base)** | **Implementation & Secure Coding**<br>*(Booking & GrabPay Source Code)* | **Executor:** Backend Microservice Developer | Defensive Coding & Pair-Review Checklist | **Constructive Code Construction**<br>*(Clean, Modular, Testable Codebase)* | **Prevention:** Applies TDD, pair programming, and refactoring to block concurrency bugs and technical debt at the source. |

---

## 3.2 Quality Attribute & Technical Constraint Verification vs. Validation

Before production release, quality assurance separates **Verification** (*"Did we build the system right?"*—static design inspections, code reviews, static analysis, and controlled test executions confirming adherence to technical specifications) from **Validation** (*"Did we build the right system?"*—empirical, operational outcomes proving the integrated system achieves real-world SLA targets and stakeholder security expectations).

### 1. Non-Functional Quality Attribute (Chosen Internally): End-to-End Checkout & Slot-Lock Response Time (`< 2.0 seconds`)

* **Verification (Engineering Activities):**
  1. **Architectural & Query Plan Inspection:** The Tech Lead reviews the *Microservice & API Architecture Blueprint* and database execution plans (`EXPLAIN ANALYZE`) to verify that 3-minute slot-lock queries use composite indexes (preventing full-table scans) and non-blocking asynchronous events for QR pass and receipt delivery.
  2. **CI Component Latency Profiling:** The Backend Microservice Developer runs automated API benchmarks inside the GitHub Actions CI pipeline to verify that internal microservice execution, lock acquisition, and wallet ledger commits complete within an internal latency budget of `< 350 ms`, preserving headroom for external GrabPay gateway round-trips.
  3. **Automated Peak-Load Execution:** Using k6/JMeter load scripts, the QA Automation Engineer simulates peak campus booking surges (5,000 concurrent student booking-and-payment requests per minute) against the staging cluster to measure response-time percentiles under lock contention.
* **Validation (Pre-Release & Canary Outcomes):**
  1. **Staging SLA Attainment Outcome:** Under sustained peak-load simulation in a production-mirrored staging environment, **at least 99% (p99) of completed booking-and-GrabPay transactions finish in strictly under `2.0 seconds`**, with zero HTTP `504` timeouts or database deadlocks.
  2. **Canary Telemetry Sign-Off Outcome:** During a controlled 5% canary rollout monitored by the DevOps Lead, live Application Performance Monitoring (APM) dashboards confirm that real-user mobile checkout and QR pass generation stay below `2.0 seconds` across 4G/5G networks without triggering duplicate client payment retries.

### 2. Technical Constraint (Mandated Externally): Local Biometric Multi-Factor Authentication (MFA) Integration

To comply with external mobile OS security models (iOS Face ID/Touch ID and Android `BiometricPrompt` via FIDO2/WebAuthn) and digital wallet governance, high-value GrabPay settlements or wallet top-ups (exceeding RM 250) require **Local Biometric MFA** before funds are debited.

* **Verification (Engineering Activities):**
  1. **Cryptographic Protocol & Contract Inspection:** The Tech Lead and DevSecOps / QA Lead inspect the authentication sequence design to verify that raw biometric templates never leave the device's Secure Enclave / Trusted Execution Environment (TEE), and that the backend verifies single-use challenge nonces, device public-key signatures, and TLS 1.3 channel binding.
  2. **SAST & SPEM Peer Code Inspection:** During Task 1's SPEM 2.0 Pull Request gate, automated SAST scanners and the Senior Peer Code Reviewer audit the authentication handler to verify the absence of hardcoded bypass flags, replayable tokens, or insecure fallback routes.
  3. **Negative & Boundary Security Test Execution:** The QA Automation Engineer executes automated negative test cases covering expired challenge nonces, replayed biometric assertions, mismatched device signatures, user-canceled prompts, and fallback PIN escalation.
* **Validation (Pre-Release & Operational Outcomes):**
  1. **Cross-Device Biometric UAT Outcome:** Across physical iOS and Android test devices in UAT, legitimate users authorize high-value GrabPay transactions via native biometrics with **100% cryptographic verification accuracy**, while tampered payloads, unauthenticated requests, and replayed assertions are deterministically rejected with `401/403` security responses.
  2. **Security Governance Approval Outcome:** The DevSecOps / QA Lead issues formal sign-off in the *Security & UAT Sign-off Report*, confirming that no high-value wallet deduction can execute without a verified hardware-backed biometric attestation, fulfilling platform security and Malaysian e-payment regulatory constraints.
