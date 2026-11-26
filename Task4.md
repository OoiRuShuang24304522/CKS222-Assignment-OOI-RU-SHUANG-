
# 5. Task 4: Lean Waste Auditing & Extreme Programming (XP) Practice Alignment

## 5.1 Lean Waste Audit & Remedies

This audit examines four potential wastes in the proposed GrabCampus facility booking and GrabPay checkout workflow. The examples are scenario-based risks rather than verified incidents at Grab Malaysia.

### Waste 1: Defects and Rework

**Case-specific example:** Two students attempt to reserve the same RM10 sports court slot. Without an exclusive slot lock, double booking may occur. Incorrect payment-state handling may also confirm an unpaid booking, causing corrective work.

**Why this is waste:** Correcting inconsistent booking records and repeating tests consumes effort without delivering new functionality.

**Remedy:** Apply TDD to concurrent reservations, payment outcomes and lock expiry. Automate regression tests and require peer review of booking logic. Measure defects and rework hours.

### Waste 2: Waiting

**Case-specific example:** The frontend checkout and QA testing are delayed while developers wait for the booking API or GrabPay response contract.

**Why this is waste:** Unresolved dependencies interrupt parallel development and delay feedback on the checkout workflow.

**Remedy:** Agree on the API contract before parallel development: `Available → Locked → Booked`, or `Locked → Available` after three minutes without successful payment. Use mock responses, visible task dependencies and prompt pull-request reviews. Measure blocked hours.

### Waste 3: Extra Features (Overproduction)

**Case-specific example:** Implementing loyalty points or dashboard analytics before completing the approved booking and payment stories wastes development and maintenance effort.

**Why this is waste:** Unrequested features increase the amount of code that must be tested and maintained, while essential requirements remain unfinished.

**Remedy:** Prioritize Task 2’s user stories in the Product Backlog, defer unapproved features and enforce GWT acceptance criteria and the Definition of Done. Track unapproved work.

### Waste 4: Extra Processing

**Case-specific example:** Developers repeatedly build the application and manually check payment, duplicate bookings and lock expiry after each change.

**Why this is waste:** Repeating predictable checks manually delays feedback and increases the chance of overlooking a regression.

**Remedy:** Trigger automated builds, regression tests and security scans through GitHub Actions. Retain manual exploratory testing where necessary. Measure manual testing effort and CI feedback time.

---

## 5.2 XP Engineering Practices

### Practice 1: Pair Programming

**Adoption:** Assign two developers to the slot-lock mechanism. The driver writes the reservation logic while the navigator checks race conditions, duplicate requests and failed payments. Swap roles regularly and submit the code for Task 1’s independent peer review.

**Practical example:** The navigator notices that checking availability and creating a lock separately could allow two requests to succeed. The pair implements an atomic reservation operation and tests simultaneous requests before submitting the pull request.

**Benefit:** Immediate feedback catches defects before merging, reduces rework and prevents misunderstandings that cause waiting.

### Practice 2: Test-First Programming (TDD)

**Adoption:** Follow Red → Green → Refactor. First write a failing test for a student with RM20 paying RM10; the expected outcome is a Booked slot and QR pass. Implement only enough code to pass, then improve its structure without changing behavior.

Repeat for insufficient balance (RM9.99: no confirmed booking or QR pass), concurrent reservation (second student rejected while locked), and unpaid lock expiry (Available after three minutes).

**GWT example:** Given an available slot and two simultaneous reservation requests, when both students proceed to checkout, then exactly one request obtains the lock and the other is rejected. This test verifies exclusivity rather than merely checking the expiry timer.

**Benefit:** Tests derived from Task 2’s acceptance criteria detect defects early and reduce repetitive manual regression checks.

### Practice 3: Continuous Integration (CI)

**Adoption:** Frequently integrate small changes through pull requests. GitHub Actions runs builds, unit tests, GWT-derived integration tests, static analysis and dependency scans. The DevSecOps / QA Lead and Senior Peer Code Reviewer verify results; failed required checks or critical findings block merging until resolved. Record approvals in Task 1’s sign-off log.

**Feedback example:** If a change allows an expired slot to remain locked, the automated regression test fails. The developer receives the failure report, corrects the logic and reruns the checks before merging. This prevents the defect from reaching the shared branch.

**Benefit:** Automated feedback reduces waiting, extra processing and rework while improving integration reliability.

### Lean-to-XP Alignment

Pair Programming prevents defects through immediate peer feedback; TDD builds quality into the implementation; CI reduces waiting and repetitive processing through automated checks. Backlog prioritization complements these practices by preventing unnecessary features from entering development.

---

## 5.3 GitHub Repository Evidence (Mandatory)

**Repository:** [GitHub Repository Commit History](https://github.com/OoiRuShuang24304522/CKS222-Assignment-OOI-RU-SHUANG-/commits/main/)

The screenshots show report-draft contributions from all five accounts between 17 and 24 November 2026. Task 4 was revised on 17, 18 and 20 November. Individual USM email addresses and the full multi-week history still require verification using genuine commit records.

![GitHub repository commit history (19–24 November 2026)](https://github.com/user-attachments/assets/8559e387-561e-444c-90e8-834fd7950df4)

*Figure 5.3.1: GitHub repository commit history (19–24 November 2026)*

![GitHub repository commit history (17–18 November 2026)](https://github.com/user-attachments/assets/6befde65-5345-4c79-8e93-1feb86695053)

*Figure 5.3.2: GitHub repository commit history (17–18 November 2026)*
