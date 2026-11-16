# Task 2: Requirements Quality & Agile User Stories (INVEST & GWT)
**Status:** Draft v0.1  
**Case Study:** Grab Malaysia (GrabCampus Facility & Shuttle Booking)  

---

## 2.1 Agile User Stories & INVEST Evaluation

### Background Context
To prevent informal requirement changes (like unrecorded WhatsApp messages), we selected the **GrabCampus Booking & GrabPay Checkout** feature. This connects our conceptual USM Facility Booking System to Grab Malaysia's commercial booking workflow.

---

### User Story 1: Instant Booking & GrabPay Payment
> **As a** USM student,  
> **I want** to book an available campus facility or shuttle seat and pay using GrabPay,  
> **So that** I can confirm my booking immediately without using cash.

#### INVEST Check (User Story 1)
* **Independent:** Can be built and tested on its own without waiting for other features like reward points.
* **Negotiable:** Screen design and receipt layout can still be discussed between the team and Product Owner.
* **Valuable:** Saves students time and automates payment tracking for the university.
* **Estimable:** The scope is clear (booking + GrabPay API), so developers can easily estimate the time needed.
* **Small:** Small enough to be finished within one 2-week sprint.
* **Testable:** Easy to verify—either the payment succeeds and the slot is booked, or it fails.

---

### User Story 2: Temporary Slot Locking
> **[TODO1 - rushuang]:** *Write User Story 2 about locking the selected slot for 3 minutes during payment.*

#### INVEST Check (User Story 2)
* **Independent:** *[To be added]*
* **Negotiable:** *[To be added]*
* **Valuable:** *[To be added]*
* **Estimable:** *[To be added]*
* **Small:** *[To be added]*
* **Testable:** *[To be added]*

---

## 2.2 Given-When-Then (GWT) Acceptance Criteria

### User Story 1 Acceptance Criteria

**Scenario 1.1: Successful Booking & Payment (Positive Path)**
* **Given** a USM student is logged into Grab with a GrabPay balance of RM 20.00,
* **When** the student selects an available RM 10.00 sports court slot and clicks "Pay with GrabPay",
* **Then** RM 10.00 is deducted, the slot status changes to "Booked", and a QR pass is generated.

**Scenario 1.2: Insufficient Balance (Negative/Boundary Path)**
* **[TODO2 - bingyi]:** *Write GWT scenario when GrabPay balance is RM 9.99 (less than RM 10.00).*

---

### User Story 2 Acceptance Criteria

**Scenario 2.1: Slot Locked for Other Users (Positive Path)**
* **[TODO3 - jinnhonn]:** *Write GWT scenario showing another student cannot book the same slot during the 3-minute lock.*

**Scenario 2.2: Lock Expires After Timeout (Negative/Boundary Path)**
* **[TODO4 - zhihuai]:** *Write GWT scenario showing the slot becomes "Available" again after 3 minutes without payment.*

---

### Definition of Done (DoD)
* **[TODO5 - yunshuang]:** *List 3–4 simple conditions required before closing these stories (e.g., code reviewed, GWT tests passed).*
