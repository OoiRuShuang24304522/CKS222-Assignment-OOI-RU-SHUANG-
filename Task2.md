# Task 2: Requirements Quality & Agile User Stories (INVEST & GWT)  
**Target Organization:** Grab Malaysia (GrabCampus Booking Integration)  

---

## 2.1 Agile User Stories & INVEST Evaluation

### Epic / Backlog Context
To eliminate informal requirement creep (such as ad-hoc feature requests sent over merchant or campus coordinator WhatsApp chats), the product backlog item selected for auditing is the **GrabCampus Integrated Booking & GrabPay Checkout** feature. This mirrors the core reservation and payment workflow of our conceptual USM Campus Facility Booking System, scaled to Grab Malaysia's commercial superapp environment.

---

### User Story 1: Instant Booking & GrabPay Wallet Checkout
> **As a** verified USM student,  
> **I want** to reserve an available campus shuttle seat or sports facility slot and pay directly using my GrabPay balance,  
> **So that** I can secure my reservation instantly without carrying cash or manually uploading payment receipts.

#### INVEST Quality Evaluation (User Story 1)
| Criteria | Evaluation & Justification |
| :--- | :--- |
| **Independent** | Can be developed and deployed independently of secondary features (such as loyalty points redemption or recurring weekly bookings) as long as core GrabPay API endpoints are exposed. |
| **Negotiable** | The UI layout of the slot picker and the exact payment confirmation receipt format remain open for discussion between the Product Owner, UX designer, and developers. |
| **Valuable** | Delivers direct, measurable value to students (cashless convenience) and university administrators (automated payment reconciliation). |
| **Estimable** | Clear functional scope (slot selection + GrabPay SDK integration) allows the Scrum team to estimate effort accurately using story points (e.g., 5 Story Points). |
| **Small** | Scoped strictly to single-slot booking and direct e-wallet checkout, enabling completion within a single two-week sprint. |
| **Testable** | Pass/fail outcomes are objective: either the slot status updates to "Confirmed" upon successful GrabPay deduction, or it remains "Available" on failure. |

---

### User Story 2: Temporary Concurrency Slot Locking
> **As a** verified USM student,  
> **I want** the system to temporarily lock my selected facility or shuttle slot for 3 minutes during GrabPay checkout,  
> **So that** another user cannot double-book the exact same slot while I am completing my payment authentication.

#### INVEST Quality Evaluation (User Story 2)
| Criteria | Evaluation & Justification |
| :--- | :--- |
| **Independent** | Operates at the reservation state-management layer (Redis/database lock) and can be built and tested separately from payment gateway UI changes. |
| **Negotiable** | The exact countdown duration (e.g., 3 minutes vs. 5 minutes) and countdown timer visual style can be negotiated based on user testing feedback. |
| **Valuable** | Prevents double-booking conflicts and user frustration during peak booking hours, directly addressing a critical flaw in basic manual booking systems. |
| **Estimable** | Standard distributed locking logic with a TTL (Time-To-Live) timer is well understood by backend engineers (estimated at 3 Story Points). |
| **Small** | Focuses solely on lock acquisition, countdown expiration, and lock release, fitting comfortably within one sprint. |
| **Testable** | Easily verified via automated concurrency tests simulating two simultaneous booking requests for the same slot ID. |

---

## 2.2 Given-When-Then (GWT) Acceptance Criteria & Definition of Done

### Acceptance Criteria for User Story 1 (GrabPay Checkout)

**Scenario 1.1: Positive Path — Successful Slot Reservation and Payment**
* **Given** the student is logged into the Grab app with a verified USM student profile and has a GrabPay balance of RM 20.00,
* **When** the student selects an available badminton court slot priced at RM 10.00 and confirms payment via GrabPay,
* **Then** the system deducts RM 10.00 from the GrabPay wallet, updates the slot status to "Booked", and generates a digital QR booking pass.

**Scenario 1.2: Negative / Boundary Path — Insufficient GrabPay Balance**
* **Given** the student has selected an available slot priced at RM 10.00,
* **And** the student's GrabPay wallet balance is RM 9.99 (boundary condition below required fare),
* **When** the student attempts to authorize the payment,
* **Then** the system blocks the transaction, displays an *"Insufficient GrabPay Balance"* prompt with a top-up shortcut, and does not generate a booking pass.

---

### Acceptance Criteria for User Story 2 (Temporary Concurrency Slot Locking)

**Scenario 2.1: Positive Path — Concurrent User Blocked During Active Lock**
* **Given** Student A has selected Slot #04 and entered the GrabPay checkout screen (triggering an active 3-minute lock),
* **When** Student B attempts to select Slot #04 at 01:30 into Student A's checkout window,
* **Then** the system displays Slot #04 as *"Temporarily Reserved"* to Student B and disables the "Book Now" button.

**Scenario 2.2: Negative / Boundary Path — Lock Expiration on Checkout Timeout**
* **Given** Student A holds a temporary 3-minute lock on Slot #04,
* **When** the 3-minute timer reaches `00:00` without payment confirmation from Student A,
* **Then** the system automatically expires the lock, cancels Student A's pending session, and immediately restores Slot #04 to *"Available"* for other students.

---
### 2.3 Defiantion of Done (DoD) & Concluding Integration

* **GWT Automtion** : All Given-When-Then acceptance scenarios must be translated into automated integration test scripts.
* **Coverage Thresholds** : Code modifications must achieve at least 80% unit test coverage and pass SonarQube static analysis with zero critical or blocker vulnerabilities.
* **Peer Review Sign-off** : The Pull Request (PR) must be audited and approved by a Senior Peer Code Reviewer, verifying that Redis distributed locking and state concurrency controls operate correctly without race conditions.
* **Traceability Alignment** : The completed user story must link directly to the functional requirements matrix and RACI deliverables established in Task 1.

---
### Next Commit TODOs (For Upcoming Days)
- [ ] Add a brief concluding paragraph summarizing the formal **Definition of Done (DoD)** checklist (e.g., unit tests passed, peer code review approved, GWT scenarios automated).
- [ ] Cross-check terminology with Task 1 RACI deliverables (`SRS & User Stories`).
