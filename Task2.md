# Task 2: Requirements Quality & Agile User Stories (INVEST & GWT)
**Case Study:** Grab Malaysia (GrabCampus Facility & Shuttle Booking)  

---

## 2.1 Agile User Stories & INVEST Evaluation

### Epic / Backlog Context
In ad-hoc development environments—such as our initial USM Facility Booking System project or early-stage merchant integrations—requirements are frequently communicated through informal channels like unrecorded WhatsApp messages, leading to severe scope creep, ambiguity, and late rework. To establish a disciplined agile requirements engineering process, we selected a major feature backlog item for **Grab Malaysia**: the **GrabCampus Integrated Facility & Shuttle Booking with GrabPay Checkout** module. This feature extends Grab’s commercial booking and payment ecosystem into a university portal context, allowing verified students to reserve campus facilities or shuttle seats seamlessly.

Below, this backlog item is decomposed into two high-quality Agile User Stories adhering to the standard `As a [Role], I want [Functionality], so that [Benefit]` template.

---

### User Story 1: Instant Booking & GrabPay Wallet Checkout
> **As a** verified USM student,  
> **I want** to reserve an available campus sports facility or shuttle seat and pay directly using my GrabPay e-wallet balance,  
> **So that** I can confirm my booking immediately without carrying physical cash or manually uploading payment receipts.

#### INVEST Criteria Evaluation — User Story 1
| INVEST Criterion | Detailed Evaluation & Justification |
| :--- | :--- |
| **Independent** | Encapsulates an end-to-end vertical slice (selecting an open slot and executing a standard GrabPay payment API call). It can be developed, tested, and deployed independently of secondary enhancements such as GrabRewards points redemption or recurring weekly reservations. |
| **Negotiable** | While the functional goal (cashless slot booking via GrabPay) is fixed, implementation details—such as the slot calendar UI layout, receipt notification format, and cancellation grace period—remain open for refinement between the Product Owner, UX designers, and developers during Sprint Planning. |
| **Valuable** | Delivers immediate, measurable value: students eliminate manual cash handling and queue times, while university facility administrators gain automated, real-time financial reconciliation. |
| **Estimable** | Because Grab’s internal GrabPay SDK and standard RESTful booking endpoints are well-documented, the engineering team can accurately size the technical effort (estimated at **5 Story Points**) without hidden architectural unknowns. |
| **Small** | Scoped strictly to single-slot selection and direct e-wallet checkout for verified student accounts, making it compact enough to be designed, coded, reviewed, and verified within a single two-week Scrum sprint. |
| **Testable** | Success criteria are binary and objectively measurable: given a valid student account and sufficient wallet balance, the transaction either deducts the exact fee and transitions the slot status to `"Booked"`, or rejects the transaction cleanly without altering inventory. |

---

### User Story 2: Temporary Concurrency Slot Locking During Checkout
> **As a** verified USM student,  
> **I want** the system to place a temporary 3-minute lock on my selected facility or shuttle slot once I enter the GrabPay checkout screen,  
> **So that** another student cannot double-book the exact same slot while I am completing my payment authentication.

#### INVEST Criteria Evaluation — User Story 2
| INVEST Criterion | Detailed Evaluation & Justification |
| :--- | :--- |
| **Independent** | Operates at the reservation state-management layer (using a time-bound distributed lock such as Redis TTL) and can be built and verified independently of front-end promotional banners or payment UI redesigns. |
| **Negotiable** | The exact lock duration (e.g., 3 minutes versus 5 minutes) and the visual countdown timer placement on the mobile interface can be negotiated based on usability testing and peak-hour traffic metrics. |
| **Valuable** | Directly prevents double-booking race conditions and user frustration during peak booking windows (such as Monday morning shuttle rushes), resolving a critical concurrency flaw present in basic manual booking systems. |
| **Estimable** | Implementing an atomic lock acquisition and automatic expiration mechanism is a standard backend pattern with predictable complexity, allowing developers to reliably estimate it at **3 Story Points**. |
| **Small** | Focused solely on lock creation upon checkout entry, blocking concurrent reads/writes, and releasing the lock upon timeout or payment failure—easily deliverable within one sprint. |
| **Testable** | Fully verifiable through automated concurrency integration tests that simulate two simultaneous booking requests targeting the same slot ID and assert that only one session acquires the lock. |

---

## 2.2 Given-When-Then (GWT) Acceptance Criteria & Definition of Done

To eliminate ambiguity between developers and QA engineers, each user story is accompanied by behavior-driven **Given-When-Then (GWT)** acceptance criteria covering both normal execution (positive path) and exception/boundary handling (negative path).

### Acceptance Criteria for User Story 1 (GrabPay Wallet Checkout)

* **Scenario 1.1 (Positive Path): Successful Slot Reservation and Instant GrabPay Deduction**
  * **Given** a student is logged into the Grab app with a verified USM student profile and holds an active GrabPay balance of `RM 20.00`,
  * **When** the student selects an available badminton court slot priced at `RM 10.00` and confirms payment via GrabPay,
  * **Then** the system deducts exactly `RM 10.00` from the student's GrabPay wallet, updates the slot status from `"Available"` to `"Booked"`, and generates a unique QR booking pass within 2 seconds.

* **Scenario 1.2 (Negative / Boundary Path): Transaction Blocked at Insufficient Balance Boundary**
  * **Given** a verified USM student has selected an available slot priced at `RM 10.00`,
  * **And** the student's GrabPay wallet balance is `RM 9.99` (exact boundary value `RM 0.01` below the required fare),
  * **When** the student attempts to authorize the payment,
  * **Then** the payment gateway rejects the transaction without deducting any funds, displays an *"Insufficient GrabPay Balance"* error prompt with a quick top-up button, and releases the pending reservation.

---

### Acceptance Criteria for User Story 2 (Temporary Concurrency Slot Locking)

* **Scenario 2.1 (Positive Path): Concurrent Booking Request Blocked During Active Lock**
  * **Given** Student A has selected Shuttle Seat `#04` and entered the GrabPay checkout screen, activating a 3-minute (`180-second`) temporary lock,
  * **When** Student B attempts to select Shuttle Seat `#04` at `01:30` into Student A’s checkout session,
  * **Then** the system displays Seat `#04` as *"Temporarily Reserved by Another User"* to Student B and disables the `"Proceed to Pay"` button until the lock is resolved.

* **Scenario 2.2 (Negative / Boundary Path): Automatic Lock Release Upon Checkout Timeout**
  * **Given** Student A holds an active 3-minute temporary lock on Shuttle Seat `#04`,
  * **When** the countdown timer reaches the exact boundary of `00:00` (`180 seconds` elapsed) without receiving a payment confirmation callback,
  * **Then** the system automatically invalidates Student A’s checkout session, releases the lock, and immediately restores Shuttle Seat `#04` to `"Available"` status for other students.
    
---

### 2.3 Defiantion of Done (DoD) & Concluding Integration

1. **Acceptance Criteria Verified:** All positive and negative/boundary GWT scenarios pass 100% in automated integration tests.
2. **Code Quality & Static Analysis:** Source code passes SonarQube static analysis with zero critical security vulnerabilities or blocker code smells (aligned with Task 1 SPEM 2.0 standards).
3. **Peer Code Review:** At least one Senior Peer Code Reviewer has approved the Pull Request, specifically verifying concurrency lock safety and financial transaction idempotency.
4. **Documentation Updated:** API endpoints and error codes are updated in the Notion Electronic Process Guide (EPG) and linked to the sprint release notes.
  
