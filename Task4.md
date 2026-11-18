# Task 4: Lean Waste Auditing & Extreme Programming (XP) Practice Alignment
--- 
**Case Study:** Grab Malaysia — Proposed GrabCampus Facility & Shuttle Booking with GrabPay Checkout

**Scope:** Conceptual integration of a USM Facility Booking System with a Grab-style booking and payment workflow

**Status:** Case-based process improvement proposal; validate examples against actual group-project records before submission.

**Evidence boundary:** GrabCampus is the group's proposed case-study feature, not a verified existing Grab Malaysia service. The situations below are potential failure scenarios and process risks, not claims that Grab Malaysia has experienced these incidents. Replace or supplement them with real evidence from the USM group project where available.

--- 
# 4.1 Lean Waste Audit & Remedies 
Lean Software Development aims to maximize value while eliminating activities that consume time or resources without improving the product. For the proposed GrabCampus booking workflow, the audit focuses on four explicit wastes: defects/rework, waiting, extra features (overproduction), and extra processing. 

## **Waste 1:** Defects and Rework — Double Booking or Incorrect Payment State
**Case-specific risk:** Two USM students select the same RM 10.00 sports court slot almost simultaneously. If the booking microservice fails to enforce the three-minute temporary lock automically, both requests could proceed, causing a double booking. Similarly, a payment failure might incorrectly leave the slot marked as 'Booked', requiring manual correction and retesting.

**Why this is waste:** Developers and testers must spend additional time diagnosing concurrency errors, reversing inconsistent booking states, and rerunning tests. The work does not add new user value.

**Process remedy:** Apply Test-Driven Development (TDD) to booking-state transitions and concurrency rules. Write automated tests for two simultaneous booking requests, successful payment, insufficient GrabPay balance (RM 9.99 against a RM 10.00 charge), and payment failure. Execute these tests automatically through the CI pipeline. Require the Senior Peer Code Reviewer to inspect the concurrency-lock logic before merge, following Task 1's Code Review & Static Analysis process.

**Expected improvement / measurement:** Fewer escaped double-booking defects and less time spent on corrective rework. Track failed concurrency tests, defects discovered after merge, and average defect-resolution time.

## **Waste 2:** Waiting — Frontend, Booking API, and GrabPay Integration Dependencies
**Case-specific risk:** A frontend developer completes the facility-selection screen but cannot finish the checkout flow while waiting for the booking microservice's slot-lock API or a payment-response contract. The QA Automation Engineer may also wait for a testable build before validating the Given-When-Then (GWT) scenarios.

### Summary Table: Given–When–Then (GWT) Scenarios 

| Scenario | Given (Context) | When (Action) | Then (Outcome) |
|---|---|---|---|
| **1. Frontend Waiting** | Booking API incomplete | Frontend attempts integration | Task becomes blocked |
| **2. Mock API Testing** | Backend unavailable | Developer uses mock responses | Frontend testing continues |
| **3. Slot Locking** | Slot available | Student requests checkout | Slot locked for 3 minutes |
| **4. Lock Expiration** | Slot locked, payment incomplete | 3 minutes elapse | Slot becomes available |
| **5. GrabPay Dependency** | Payment contract incomplete | Developer integrates checkout | Integration blocked |
| **6. CI Testing** | Components implemented | Developer opens PR | Automated tests execute |

**Why this is waste:** Features remain blocked while team members wait for interfaces, reviews, or test environments instead of completing valuable work.

**Process remedy:** Agree on a shared API contract for Available → Locked → Booked and Locked → Available (after three minutes without payment) before parallel implementation. Use mocked API responses for early frontend testing, make dependencies visible on a shared task board, and set an agreed turnaround target for pull-request reviews. Integrate small working changes frequently instead of waiting for a large end-of-sprint merge.

**Expected improvement / measurement:** Shorter blocked-task time and faster end-to-end integration. Record hours that tasks remain blocked, pull-request review turnaround, and time from feature completion to integration.

## **Waste 3:** Extra Features / Overproduction — Unapproved Booking Enhancements
**Case-specific risk:** Developers build loyalty points, elaborate receipt customization, or additional dashboard analytics before completing the approved booking-and-payment flow and three-minute slot-lock feature. These enhancements are outside the two prioritized Task 2 user stories.

**Why this is waste:** Unrequested features consume implementation, review, and testing effort while delaying essential functionality.

**Process remedy:** Keep a Product Manager-approved, prioritized backlog. During sprint planning, select only work that directly supports the two user stories and their GWT acceptance criteria. Route new ideas through backlog refinement rather than implementing them immediately. Use the Definition of Done (DoD) to prevent incomplete core features from being treated as finished.

**Expected improvement / measurement:** More effort spent on approved user value and less unplanned scope. Track unapproved features started, sprint work carried over, and completion of accepted user stories.

## **Waste 4:** Extra Processing — Repetitive Manual Builds and Regression Checks
**Case-specific risk:** After every change to the booking or payment microservice, team members manually rebuild the application and repeat the same checks: successful RM 10.00 GrabPay checkout, insufficient RM 9.99 balance, competing slot requests, and three-minute lock expiry. Repeating these steps by hand can delay feedback and produce inconsistent results.

**Why this is waste:** Repeated manual execution of deterministic checks adds effort without improving the functionality being delivered.

**Process remedy:** Configure GitHub Actions Continuous Integration (CI) to trigger on pull requests and relevant pushes. Automatically run unit tests, GWT-derived integration tests, build checks, static analysis, and dependency vulnerability scanning. Following Task 1, block merges with critical security findings or failed required tests, and retain the Automated Static Analysis & Security Scan Report and Code Review Approval & Sign-off Log as evidence. Manual exploratory testing remains useful and should not be eliminated.

**Expected improvement / measurement:** Less repetitive manual work, more consistent verification, and earlier detection of broken builds. Track CI run duration, failed-build detection time, and manual regression-testing effort.
---
**Lean Waste Audit Summary**
---
Lean Waste	GrabCampus Case-Specific Risk	Targeted Remedy	Suggested Indicator
Defects and rework	Double booking or inconsistent payment/booking status	TDD, automated concurrency tests, peer review	Escaped defects; rework hours
Waiting	Frontend or QA blocked by API, payment contract, or review	Shared API contracts, mocks, smaller PRs, review targets	Blocked hours; review turnaround
Extra features / overproduction	Loyalty or dashboard extras before core booking stories	Prioritized backlog, GWT acceptance criteria, DoD	Unapproved scope; carry-over work
Extra processing	Repeated manual builds and checkout/lock regression tests	GitHub Actions CI, automated tests and scans	Manual test time; CI feedback time

---

# 4.2 XP Engineering Practices
---
Extreme Programming (XP) emphasizes close collaboration, automated testing, and frequent integration to improve code quality and shorten feedback loops. The following three practices are tailored to the same GrabCampus Booking & GrabPay Checkout feature and support Task 1's SPEM-based review workflow and Task 2's user stories.
**Practice 1:** Pair Programming
**Definition:** Two developers collaborate on one programming task. The driver writes code, while the navigator continuously reviews logic, identifies edge cases, and considers design quality. They regularly swap roles.

**Proposed adoption:**
1. Assign a Backend Microservice Developer and a second developer/reviewer to the high-risk three-minute slot-lock and GrabPay transaction-state logic.
2. Work together on an atomic reservation operation that prevents two students from claiming the same slot.
3. Swap driver and navigator roles at agreed intervals; discuss lock expiry, duplicate requests, and failed payment handling.
4. Commit the reviewed code and unit tests to a feature branch and open a Feature Pull Request (PR).
5. Continue the formal Senior Peer Code Reviewer and DevSecOps / QA Lead approval process described in Task 1. Pair programming supports, but does not replace, the required independent PR quality gate.
Example: The driver implements the rule that a second student cannot reserve a slot already locked by another student. The navigator checks for race conditions and confirms that an unpaid lock expires after three minutes.

**Lean alignment:** Primarily reduces defects and rework through immediate feedback and shared understanding; it may also reduce waiting caused by misunderstood interfaces.

**Evidence / indicator:** Pair-session notes or linked PRs; concurrency defects found before merge; review rework count.
**Practice 2:** Test-First Programming / Test-Driven Development (TDD)
Definition: TDD follows the Red → Green → Refactor cycle: write a failing automated test, implement only enough code to pass it, then improve the design without breaking tests.

Proposed adoption using Task 2 acceptance criteria:
1. Red: Write a failing test for Task 2's successful checkout: a logged-in student with RM 20.00 pays RM 10.00 for an available sports court, the slot becomes Booked, and a QR pass is generated.
2. Green: Implement the minimum booking and payment logic needed to satisfy that test.
3. Refactor: Improve the booking service's code structure while keeping the test passing.
4. Repeat for the RM 9.99 insufficient-balance case, simultaneous attempts to book the same slot, and three-minute unpaid lock expiry.
5. Run the complete test suite in CI before the PR can be approved.
Illustrative test matrix (expected behavior to confirm with Task 2's final GWT/DoD):
Test	Input / Condition	Expected Result
TDD-01: Successful payment	Balance RM 20.00; slot RM 10.00	Payment succeeds; slot Booked; QR pass generated
TDD-02: Insufficient balance	Balance RM 9.99; slot RM 10.00	Payment rejected; no confirmed booking or QR pass
TDD-03: Concurrent reservation	Student A holds an active three-minute lock	Student B cannot acquire the same slot during the lock
TDD-04: Lock timeout	Three minutes elapse without successful payment	Slot returns to Available, subject to final agreed rules


**Lean alignment:** Directly reduces defects and rework and avoids repeated manual checks under extra processing.

**Evidence / indicator:** Test commits preceding implementation commits, CI test results, regression pass rate, and defects discovered before merge.
**Practice 3:** Continuous Integration (CI)
Definition: Developers frequently integrate small changes into a shared repository. Automated builds, tests, and quality checks provide rapid feedback on each proposed change.

Proposed adoption aligned with Task 1's SPEM workflow:
1. The Software Engineer / Backend Microservice Developer creates a feature branch for a Task 2 user story and opens a Feature PR.
2. A GitHub Actions workflow triggers on the PR and executes the application build, automated unit/integration tests, static code analysis, and dependency vulnerability checks.
3. The DevSecOps / QA Lead checks the Automated Static Analysis & Security Scan Report. Failed required tests or critical/blocker findings prevent merging.
4. The Senior Peer Code Reviewer checks concurrency locks, payment-state integrity, and maintainability; the developer fixes requested changes.
5. Once required checks and approvals pass, the Code Review Approval & Sign-off Log is recorded and the PR can merge into the designated integration/staging branch.
6. The QA Automation Engineer verifies the GWT scenarios; the DevOps Engineer maintains the pipeline and release process according to Task 1's RACI responsibilities.
Illustrative feedback loop:

Code change → Feature PR → Automated build and tests → Static/security scan → Peer review → Fix if needed → Approval and merge

**Lean alignment:** Reduces extra processing by automating repeatable checks, waiting by providing faster integration feedback, and defects and rework by catching failures before merge.

**Evidence / indicator:** GitHub Actions run logs, test and scan reports, PR approval records, failed-build detection time, and integration frequency.
Explicit Lean-to-XP Alignment

## Reference link
https://www.mindbowser.com/devops-ci-cd-pipeline-stages/

XP Practice	Waste(s) Addressed	How It Shortens Feedback / Improves Quality
Pair Programming	Defects and rework; some waiting	Navigator identifies booking concurrency and payment-state issues while code is written
TDD	Defects and rework; extra processing	GWT-derived tests expose incorrect behavior before implementation is considered complete
CI	Extra processing; waiting; defects and rework	Automated builds, tests, and scans flag failures on each PR instead of at final integration


Integration with teammates' work: Task 2 defines what correct behavior means through user stories and GWT scenarios; Task 4 turns that behavior into TDD tests and rapid feedback; Task 1 provides the formal PR, security scanning, peer review, sign-off, and RACI controls that govern merging.
