# Task 2: Requirements Quality & Agile User Stories (INVEST & GWT)
**Case Study:** Grab Malaysia (GrabCampus Facility & Shuttle Booking)  

---

## 2.1 Agile User Stories & INVEST Evaluation

### Background Context
To eliminate informal requirement creep—such as ad-hoc changes sent over unrecorded merchant or campus coordinator WhatsApp chats—we selected the **GrabCampus Integrated Facility & Shuttle Booking with GrabPay Checkout** feature. This connects our conceptual USM Facility Booking System to Grab Malaysia's commercial booking and payment workflow.

---

### User Story 1: Instant Booking & GrabPay Wallet Checkout
> **As a** verified USM student,  
> **I want** to reserve an available campus facility or shuttle seat and pay directly using my GrabPay balance,  
> **So that** I can confirm my booking immediately without carrying cash or manually uploading receipts.

#### INVEST Evaluation (User Story 1)
| Criteria | Evaluation & Justification |
| :--- | :--- |
| **Independent** | Can be developed and deployed independently of secondary features (such as GrabRewards redemption or recurring bookings). |
| **Negotiable** | The UI layout of the slot picker and the receipt notification format remain open for discussion between the Product Owner and developers. |
| **Valuable** | Saves students time through cashless booking and automates payment tracking for university administrators. |
| **Estimable** | Clear functional scope (slot selection + GrabPay API integration) allows the team to estimate effort accurately (e.g., 5 Story Points). |
| **Small** | Scoped strictly to single-slot booking and direct e-wallet checkout, fitting within a single two-week sprint. |
| **Testable** | Pass/fail outcomes are objective: either GrabPay deducts the fee and marks the slot "Booked", or the transaction is rejected. |

---

### User Story 2: Temporary Concurrency Slot Locking
> **As a** verified USM student,  
> **I want** the system to temporarily lock my selected facility or shuttle slot for 3 minutes during GrabPay checkout,  
> **So that** another student cannot double-book the exact same slot while I am completing payment authentication.

#### INVEST Evaluation (User Story 2)
| Criteria | Evaluation & Justification |
| :--- | :--- |
| **Independent** | Operates at the reservation state-management layer (distributed lock) and can be built separately from payment UI changes. |
| **Negotiable** | The exact lock duration (e.g., 3 minutes vs. 5 minutes) and countdown timer UI placement can be negotiated based on user testing. |
| **Valuable** | Prevents double-booking race conditions during peak booking windows, resolving a key flaw in manual booking systems. |
| **Estimable** | Standard lock acquisition and TTL (Time-To-Live) expiration logic is well understood by backend engineers (estimated at 3 Story Points). |
| **Small** | Focuses solely on lock creation, blocking concurrent requests, and releasing on timeout—achievable within one sprint. |
| **Testable** | Verifiable via automated concurrency tests simulating two simultaneous booking requests for the same slot ID. |

---

## 2.2 Given-When-Then (GWT) Acceptance Criteria & Definition of Done

### User Story 1 Acceptance Criteria (GrabPay Checkout)

**Scenario 1.1: Successful Booking & Payment (Positive Path)**
* **Given** a verified USM student is logged into Grab with a GrabPay balance of `RM 20.00`,
* **When** the student selects an available badminton court slot priced at `RM 10.00` and confirms payment via GrabPay,
* **Then** `RM 10.00` is deducted from GrabPay, the slot status changes to `"Booked"`, and a digital QR pass is generated.

**Scenario 1.2: Insufficient Balance Boundary (Negative/Boundary Path)**
* **Given** a verified USM student has selected an available slot priced at `RM 10.00`,
* **And** the student's GrabPay balance is `RM 9.99` (exact boundary value `RM 0.01` below the fare),
* **When** the student attempts to authorize payment,
* **Then** the system rejects the transaction without deducting funds, displays an *"Insufficient GrabPay Balance"* prompt, and leaves the slot available.

---

### User Story 2 Acceptance Criteria (Temporary Slot Locking)

**Scenario 2.1: Concurrent User Blocked During Active Lock (Positive Path)**
* **Given** Student A has selected Shuttle Seat `#04` and entered the GrabPay checkout screen (triggering an active 3-minute lock),
* **When** Student B attempts to select Seat `#04` at `01:30` into Student A's checkout window,
* **Then** the system displays Seat `#04` as *"Temporarily Reserved"* to Student B and disables the `"Proceed to Pay"` button.

**Scenario 2.2: Automatic Lock Expiration on Timeout (Negative/Boundary Path)**
* **Given** Student A holds a temporary 3-minute lock on Shuttle Seat `#04`,
* **When** the countdown timer reaches `00:00` (`180 seconds`) without receiving payment confirmation,
* **Then** the system cancels Student A's pending session, releases the lock, and restores Seat `#04` to `"Available"` status.

---

---
### 2.3 Defiantion of Done (DoD) & Concluding Integration

* **GWT Automtion** : All Given-When-Then acceptance scenarios must be translated into automated integration test scripts.
* **Coverage Thresholds** : Code modifications must achieve at least 80% unit test coverage and pass SonarQube static analysis with zero critical or blocker vulnerabilities.
* **Peer Review Sign-off** : The Pull Request (PR) must be audited and approved by a Senior Peer Code Reviewer, verifying that Redis distributed locking and state concurrency controls operate correctly without race conditions.
* **Traceability Alignment** : The completed user story must link directly to the functional requirements matrix and RACI deliverables established in Task 1.
  
* *[TODO - Final Polish]: Refine DoD wording and align terminology with Task 1 SPEM 2.0 & RACI deliverables before final submission.*
