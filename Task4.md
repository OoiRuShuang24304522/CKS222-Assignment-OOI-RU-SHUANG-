
# Task 4: Lean Waste Auditing & Extreme Programming (XP) Practice Alignment

**Case Study:** Grab Malaysia — Proposed GrabCampus Facility & Shuttle Booking with GrabPay Checkout

**Scope:** Conceptual integration of a USM Facility Booking System with a Grab-style booking and payment workflow.

**Status:** Case-based process improvement proposal.

**Evidence Boundary:** GrabCampus is the group's proposed case-study feature, not a verified existing Grab Malaysia service. The examples below describe potential development risks and proposed improvements rather than confirmed incidents within Grab Malaysia.

---

## 4.1 Lean Waste Audit & Remedies

Lean Software Development aims to maximize customer value by eliminating activities that consume time and resources without improving the software product.

For the proposed GrabCampus booking system, four Lean wastes are identified: **Defects and Rework, Waiting, Extra Features (Overproduction), and Extra Processing**.

### Waste 1: Defects and Rework — Double Booking or Incorrect Payment State

**Case-specific risk:**

Two USM students attempt to book the same RM 10.00 sports court slot at almost the same time. If the booking microservice does not properly enforce the temporary slot-lock mechanism, both students might successfully reserve the same slot, resulting in double booking.

Similarly, if a GrabPay transaction fails but the system still marks the slot as "Booked", developers must investigate the error and correct the booking status.

**Why this is waste:**

Developers and testers must spend additional time identifying concurrency errors, correcting booking records, and repeating tests. These activities increase development effort without adding new value to the system.

**Process remedy:**

Apply Test-Driven Development (TDD) to booking-state transitions and concurrency rules.

Developers should create automated tests for:

1. Two students attempting to book the same slot simultaneously.
2. Successful GrabPay payment.
3. Insufficient GrabPay balance (RM 9.99 for a RM 10.00 booking).
4. Failed payment transactions.
5. Three-minute slot-lock expiration.

These tests should run automatically through the Continuous Integration (CI) pipeline.

Following Task 1's Code Review & Static Analysis process, the Senior Peer Code Reviewer should also verify the concurrency-lock logic before approving a pull request.

**Expected improvement / measurement:**

Earlier detection of booking errors, fewer double-booking incidents, and reduced debugging time.

Suggested indicators include:

- Number of concurrency-related defects.
- Defects discovered after merging.
- Average defect-resolution time.
- Hours spent on corrective rework.

---

### Waste 2: Waiting — Frontend, Booking API, and GrabPay Integration Dependencies

**Case-specific risk:**

A frontend developer completes the facility-selection interface but cannot finish the checkout workflow because the booking microservice's slot-lock API is still under development.

The developer may also need to wait for the GrabPay payment-response contract to be finalized.

Similarly, the QA Automation Engineer may be unable to validate the Given–When–Then (GWT) acceptance criteria until a testable build becomes available.

#### Summary Table: Given–When–Then (GWT) Scenarios

| Scenario | Given (Context) | When (Action) | Then (Outcome) |
|---|---|---|---|
| **1. Frontend Waiting** | The frontend interface is completed, but the booking API is still under development. | The frontend developer attempts to connect the interface to the booking service. | Integration is delayed because the required API is unavailable. |
| **2. Mock API Testing** | The backend API is incomplete, but the team has agreed on the API contract. | The frontend developer uses mock API responses to simulate booking operations. | Frontend development and testing can continue without waiting for the backend. |
| **3. Slot Locking** | A student selects an available RM 10.00 sports court slot. | The student proceeds to checkout and requests a temporary slot lock. | The slot changes from "Available" to "Locked" for three minutes, preventing other students from booking it. |
| **4. Lock Expiration** | A student has locked a slot but has not completed payment. | Three minutes pass without successful payment. | The system releases the lock and changes the slot status back to "Available". |
| **5. GrabPay Dependency** | The checkout interface is ready, but the payment-response contract has not been finalized. | The developer attempts to integrate GrabPay with the booking workflow. | Integration is delayed because the system cannot reliably process payment responses. |
| **6. CI Testing** | The frontend and backend components have been implemented and automated tests are configured. | A developer submits a pull request to GitHub. | The CI pipeline automatically runs the required build and integration tests, providing early feedback. |

Scenarios 1 and 5 demonstrate waiting waste, while Scenarios 2 and 6 illustrate how development delays can be reduced. Scenarios 3 and 4 describe the booking behavior that the shared API contract must support.

**Why this is waste:**

Developers and testers cannot complete their assigned tasks while waiting for APIs, reviews, or test environments. These delays reduce productivity and slow down software integration.

**Process remedy:**

The team should agree on a shared API contract before implementing the frontend and backend components separately.

The contract should define the following booking-state transitions:

- `Available → Locked`: A student selects a slot and begins checkout.
- `Locked → Booked`: The student successfully completes payment.
- `Locked → Available`: The three-minute lock expires without successful payment.

Additional improvements include:

1. Using mock API responses to support early frontend development.
2. Recording task dependencies on a shared task board.
3. Establishing an agreed turnaround target for pull-request reviews.
4. Integrating small code changes regularly rather than waiting until the end of a sprint.

**Expected improvement / measurement:**

Reduced waiting time, smoother collaboration, and faster integration between frontend, booking, and payment components.

Suggested indicators include:

- Number of hours development tasks remain blocked.
- Average pull-request review turnaround time.
- Time between feature completion and successful integration.

---

### Waste 3: Extra Features / Overproduction — Unapproved Booking Enhancements

**Case-specific risk:**

Developers may begin implementing loyalty points, additional dashboard analytics, or advanced receipt customization before completing the core booking and payment functionality.

These features are not included in the two prioritized user stories defined in Task 2.

**Why this is waste:**

Developing unnecessary features consumes time that could be spent completing essential requirements. It also creates additional code that must be reviewed, tested, and maintained.

**Process remedy:**

Maintain a prioritized Product Backlog approved by the Product Manager.

During sprint planning, developers should focus on features that directly support Task 2's user stories and GWT acceptance criteria.

New feature suggestions should be recorded for future backlog refinement rather than implemented immediately.

The team should also follow the Definition of Done (DoD) to ensure that essential functionality is fully tested and reviewed before a user story is considered complete.

**Expected improvement / measurement:**

Better use of development resources, fewer unnecessary features, and improved focus on approved requirements.

Suggested indicators include:

- Number of unapproved features started.
- Amount of unfinished work carried into the next sprint.
- Number of accepted user stories completed.

---

### Waste 4: Extra Processing — Repetitive Manual Builds and Regression Checks

**Case-specific risk:**

After modifying the booking or payment microservice, developers repeatedly rebuild the application and manually perform the same tests.

These tests may include:

1. Successful RM 10.00 GrabPay checkout.
2. Insufficient GrabPay balance of RM 9.99.
3. Two students attempting to reserve the same slot.
4. Three-minute slot-lock expiration.

Repeating these checks manually after every code change can delay feedback and produce inconsistent testing results.

**Why this is waste:**

Repetitive manual testing consumes development time without adding new functionality. Human errors may also occur when the same checks are performed repeatedly.

**Process remedy:**

Implement Continuous Integration using GitHub Actions.

The CI pipeline should automatically execute the following checks whenever a pull request is opened or relevant code changes are pushed:

1. Application build verification.
2. Automated unit testing.
3. GWT-derived integration testing.
4. Static code analysis.
5. Dependency vulnerability scanning.

Following Task 1's proposed process, pull requests with failed required tests or critical security findings should not be merged.

The team should also retain the Automated Static Analysis & Security Scan Report and Code Review Approval & Sign-off Log.

Manual exploratory testing should still be performed when human judgment is necessary.

**Expected improvement / measurement:**

Less repetitive manual work, more consistent testing results, and earlier detection of software defects.

Suggested indicators include:

- CI pipeline execution time.
- Time required to detect failed builds.
- Hours spent on manual regression testing.

---

### Lean Waste Audit Summary

| Lean Waste | GrabCampus Case-Specific Risk | Targeted Remedy | Suggested Indicator |
|---|---|---|---|
| **Defects and Rework** | Double booking or incorrect payment and booking status | TDD, automated concurrency tests, and peer code review | Number of defects and rework hours |
| **Waiting** | Frontend or QA developers waiting for APIs, payment contracts, or code reviews | Shared API contracts, mock responses, smaller pull requests, and review targets | Blocked hours and review turnaround time |
| **Extra Features / Overproduction** | Developing loyalty points or dashboard features before completing core booking requirements | Prioritized Product Backlog, GWT acceptance criteria, and Definition of Done (DoD) | Unapproved features and unfinished sprint tasks |
| **Extra Processing** | Repeated manual builds and regression testing for checkout and slot-lock functions | GitHub Actions CI, automated testing, and security scanning | Manual testing time and CI feedback time |

---

## 4.2 Extreme Programming (XP) Engineering Practices

Extreme Programming (XP) is an Agile software development approach that emphasizes teamwork, automated testing, continuous feedback, and frequent integration.

For the proposed GrabCampus system, three XP practices are recommended: **Pair Programming, Test-First Programming (TDD), and Continuous Integration (CI)**.

These practices support Task 1's code review and static analysis workflow while helping developers satisfy Task 2's user stories and acceptance criteria.

### Practice 1: Pair Programming

**Definition:**

Pair Programming involves two developers working together on the same programming task.

- **Driver:** Writes the source code.
- **Navigator:** Reviews the code, identifies possible errors, and suggests improvements.

Both developers regularly exchange roles to improve collaboration and share technical knowledge.

**Proposed adoption:**

1. Assign two developers to work on complex booking and payment functionality.
2. Allow the driver to implement the three-minute slot-lock mechanism while the navigator reviews the logic.
3. Exchange roles regularly and discuss potential problems such as duplicate bookings, failed payments, and lock expiration.
4. Commit the completed code and unit tests to a feature branch.
5. Open a GitHub pull request for formal review.

Following Task 1, the Senior Peer Code Reviewer and DevSecOps / QA Lead should still complete the required review and approval process.

Pair Programming supports the review process but does not replace the formal quality gate.

**Example:**

The driver implements a booking rule that prevents a second student from reserving a slot already locked by another student.

Meanwhile, the navigator checks for possible race conditions and verifies that the system releases the slot after three minutes without successful payment.

**Lean alignment:**

Pair Programming mainly reduces Defects and Rework because developers can identify problems while writing code.

It may also reduce Waiting by helping developers clarify technical requirements together.

**Evidence / measurement:**

- Pair-programming session records.
- Pull requests linked to collaborative work.
- Number of defects identified before merging.
- Number of corrections requested during code review.

---

### Practice 2: Test-First Programming / Test-Driven Development (TDD)

**Definition:**

Test-Driven Development requires developers to write automated tests before implementing the corresponding functionality.

TDD follows the **Red → Green → Refactor** cycle.

**Proposed adoption using Task 2 acceptance criteria:**

1. **Red:** Developers first write an automated test for the booking and payment requirements. For example, a student with RM 20.00 in GrabPay should be able to pay RM 10.00 for an available sports court slot. The test initially fails because the required functionality has not been implemented.

2. **Green:** Developers write the minimum amount of code needed to make the test pass. Once payment is successful, the system should mark the slot as "Booked" and generate a QR pass.

3. **Refactor:** Developers improve the code structure and readability without changing the expected results.

4. **Repeat:** The team applies the same process to other scenarios, including insufficient GrabPay balance (RM 9.99), simultaneous booking attempts, and three-minute slot-lock expiration.

5. **Continuous Testing:** All automated tests are executed through the CI pipeline whenever developers submit code changes.

**Expected improvement:**

Writing tests before implementation helps developers identify problems earlier, reduce repeated debugging, and maintain consistent booking and payment functionality.

#### Illustrative TDD Test Matrix

| Test Case | Input / Condition | Expected Result |
|---|---|---|
| **TDD-01: Successful Payment** | GrabPay balance is RM 20.00 and the booking costs RM 10.00. | Payment succeeds, the slot becomes "Booked", and a QR pass is generated. |
| **TDD-02: Insufficient Balance** | GrabPay balance is RM 9.99, but the booking costs RM 10.00. | Payment is rejected, and no confirmed booking or QR pass is generated. |
| **TDD-03: Concurrent Reservation** | Student A has already locked the selected slot for three minutes. | Student B cannot book the same slot while the lock remains active. |
| **TDD-04: Lock Expiration** | A student has locked a slot but does not complete payment within three minutes. | The slot automatically returns to "Available". |

These test cases are based on the proposed GrabCampus workflow and should be checked against the finalized GWT acceptance criteria in Task 2.

**Lean alignment:**

TDD reduces Defects and Rework by identifying incorrect behavior early.

It also reduces Extra Processing by providing automated regression tests that can be executed repeatedly without requiring the same manual checks.

**Evidence / measurement:**

- Automated test cases.
- Test commits created before implementation.
- CI test execution results.
- Regression test pass rate.
- Defects identified before merging.

---

### Practice 3: Continuous Integration (CI)

**Definition:**

Continuous Integration is an XP engineering practice in which developers frequently integrate small code changes into a shared repository.

Automated builds, tests, and quality checks help identify errors before they affect other parts of the system.

**Proposed adoption aligned with Task 1's SPEM workflow:**

1. The Software Engineer or Backend Microservice Developer creates a feature branch for a Task 2 user story and opens a GitHub pull request.

2. GitHub Actions automatically executes application builds, unit tests, integration tests, static code analysis, and dependency vulnerability checks.

3. The DevSecOps / QA Lead reviews the Automated Static Analysis & Security Scan Report. Pull requests containing failed required tests or critical security findings cannot be merged until the issues are resolved.

4. The Senior Peer Code Reviewer examines booking concurrency rules, payment-state handling, and code maintainability.

5. Developers correct any identified issues and submit updated commits.

6. Once the required checks and approvals pass, the Code Review Approval & Sign-off Log is recorded, and the pull request can be merged into the designated integration or staging branch.

7. The QA Automation Engineer verifies the GWT acceptance criteria, while the DevOps Engineer maintains the CI pipeline according to Task 1's RACI responsibilities.

#### Illustrative CI Feedback Loop

```text
Code Change
    ↓
GitHub Feature Pull Request
    ↓
Automated Build and Tests
    ↓
Static Analysis and Security Scan
    ↓
Peer Code Review
    ↓
Fix Identified Issues (If Required)
    ↓
Approval and Merge
```

**Lean alignment:**

Continuous Integration addresses several Lean wastes:

- **Extra Processing:** Automates repetitive builds and regression tests.
- **Waiting:** Provides faster feedback and reduces integration delays.
- **Defects and Rework:** Detects software errors before code changes are merged.

**Expected improvement:**

Developers receive earlier feedback, reduce repetitive testing, and maintain more reliable integration between the booking and payment components.

**Evidence / measurement:**

- GitHub Actions execution logs.
- Automated test results.
- Static analysis and security scan reports.
- Pull-request approval records.
- Failed-build detection time.
- Frequency of successful code integration.

---

### Explicit Lean-to-XP Alignment

| XP Practice | Lean Waste Addressed | How It Improves Software Development |
|---|---|---|
| **Pair Programming** | Defects and Rework; Waiting | Two developers work together to identify booking errors and concurrency issues earlier, reducing misunderstandings and repeated corrections. |
| **Test-Driven Development (TDD)** | Defects and Rework; Extra Processing | Automated tests verify booking and payment requirements before implementation is considered complete, reducing debugging and repetitive manual testing. |
| **Continuous Integration (CI)** | Extra Processing; Waiting; Defects and Rework | Automated builds, tests, and security scans provide early feedback whenever developers submit changes, reducing integration delays and manual checking. |

### Integration with Tasks 1 and 2

The proposed XP practices are connected to the earlier assignment tasks.

**Task 1: Process Definition & Modeling**

Task 1 defines the formal Code Review & Static Analysis workflow, including pull requests, automated security scanning, peer code review, approval records, and RACI responsibilities.

Task 4 uses these process controls to support Pair Programming, TDD, and Continuous Integration.

**Task 2: Requirements Quality & Agile User Stories**

Task 2 defines the expected booking and payment behavior through Agile User Stories, INVEST evaluation, and Given–When–Then acceptance criteria.

Task 4 uses these requirements to identify potential development waste and propose automated tests for successful payment, insufficient balance, concurrent bookings, and slot-lock expiration.

Together, the three tasks connect requirements, software development practices, quality assurance, and process improvement.

---


## 4.3 GitHub Repository Evidence (Mandatory)

GitHub is used to manage the assignment documentation, track individual contributions, and support collaboration among all five group members.

The repository's commit history provides evidence of continuous contributions, document revisions, and improvements made throughout the assignment.

### 4.3.1 Repository Information

**GitHub Repository:** [CKS222 Assignment Repository](https://github.com/OoiRuShuang24304522/CKS222-Assignment-OOI-RU-SHUANG-)

**Branch:** `main`

**Commit History:** [View GitHub Commit History](https://github.com/OoiRuShuang24304522/CKS222-Assignment-OOI-RU-SHUANG-/commits/main/)

**Evidence Period:** 17 November 2026 – 24 November 2026

**Number of Contributors Observed:** 5 group members

### 4.3.2 GitHub Commit History Screenshots

The following screenshots show the group's actual GitHub commit history, including contributor usernames, commit messages, commit hashes, and contribution dates.

#### Figure 1: GitHub Commit History (19–24 November 2026)

<img width="1920" height="1200" alt="Screenshot (43)" src="https://github.com/user-attachments/assets/8559e387-561e-444c-90e8-834fd7950df4" />

*Figure 1: GitHub commit history showing contributions and revisions made between 19 November and 24 November 2026.*

#### Figure 2: GitHub Commit History (17–18 November 2026)

<img width="1920" height="1200" alt="Screenshot (44)" src="https://github.com/user-attachments/assets/6befde65-5345-4c79-8e93-1feb86695053" />

*Figure 2: GitHub commit history showing contributions and revisions made between 17 November and 18 November 2026.*

### 4.3.3 Commit History Summary

The following table summarizes the fully visible commit records from the attached GitHub screenshots.

| Date | GitHub Contributor | Commit Message | Commit Hash |
|---|---|---|---|
| 24 Nov 2026 | OoiRuShuang24304522 | Revise Task1.md for improved clarity and structure | `8f6f0e3` |
| 21 Nov 2026 | KuYunShuang24304022 | Revise user stories and acceptance criteria for clarity | `0e45631` |
| 20 Nov 2026 | CHIAMZHIHUAI24303835 | Revise Lean Waste Audit and XP Practices for GrabCampus | `646cd1c` |
| 20 Nov 2026 | OoiRuShuang24304522 | Revise roles, tasks, and work products in Task1.md | `41c807f` |
| 20 Nov 2026 | LIMBINGYI24304515 | Enhance V-Model design with GrabCampus case study | `107eda6` |
| 19 Nov 2026 | CheahJinnHonn24303397 | Update Task5.md | `46b2c89` |
| 19 Nov 2026 | CheahJinnHonn24303397 | Update Task5.md | `e6c570d` |
| 19 Nov 2026 | KuYunShuang24304022 | Expanded INVEST evaluation into tables and added GWT scenarios | `fa104b9` |
| 18 Nov 2026 | LIMBINGYI24304515 | Expand Task 3 with V-Model details and mapping | `caf7968` |
| 18 Nov 2026 | CheahJinnHonn24303397 | Update Task5.md | `40e65d7` |
| 18 Nov 2026 | CHIAMZHIHUAI24303835 | Revise Task4.md for clarity and structure improvements | `2a8df24` |
| 17 Nov 2026 | LIMBINGYI24304515 | Fix header formatting in Task3.md | `3e59938` |
| 17 Nov 2026 | LIMBINGYI24304515 | Enhance Task 3 with V-Model design explanation | `83d9c2f` |
| 17 Nov 2026 | KuYunShuang24304022 | Revise user stories and acceptance criteria | `944d57f` |
| 17 Nov 2026 | CHIAMZHIHUAI24303835 | Update Task4.md | `529cdd3` |
| 17 Nov 2026 | CHIAMZHIHUAI24303835 | Update Task4.md | `5d40380` |
| 17 Nov 2026 | CHIAMZHIHUAI24303835 | Update header formatting in Task4.md | `4fc04b5` |
| 17 Nov 2026 | CHIAMZHIHUAI24303835 | Document Lean Waste Audit and XP Practices for GrabCampus | `fb01898` |
| 17 Nov 2026 | CheahJinnHonn24303397 | Update Task1.md | `238110b` |

**Note:** This table is a transcription of 19 fully visible commit records from the screenshots. It is not the complete repository history or an original Git log.

### 4.3.4 Individual Contribution Summary

| Group Member | Matric Number | GitHub Username | Observed Contribution |
|---|---|---|---|
| Ooi Ru Shuang | 24304522 | `OoiRuShuang24304522` | Task 1: Process Definition & Modeling, including roles, tasks, work products, and documentation improvements. |
| Ku Yun Shuang | 24304022 | `KuYunShuang24304022` | Task 2: Agile User Stories, INVEST evaluation, and GWT acceptance criteria. |
| Lim Bing Yi | 24304515 | `LIMBINGYI24304515` | Task 3: V-Model design, explanation, mapping, and documentation improvements. |
| Chiam Zhi Huai | 24303835 | `CHIAMZHIHUAI24303835` | Task 4: Lean Waste Audit, XP Engineering Practices, and documentation improvements. |
| Cheah Jinn Honn | 24303397 | `CheahJinnHonn24303397` | Task 5 documentation updates and additional contributions to Task 1. |

### 4.3.5 Evidence Analysis

The GitHub commit history shows that all five group members contributed to the assignment through their respective GitHub accounts.

The recorded contributions include initial documentation, content expansion, technical explanations, structural improvements, and revisions to existing assignment sections.

The screenshots show contributions between 17 November and 24 November 2026, rather than a single final submission.

For example, Task 4 was initially documented on 17 November 2026 and subsequently revised on 18 November and 20 November 2026. These revisions demonstrate an iterative approach to improving the clarity, structure, and technical content of the assignment.

The commit history also shows that group members contributed to different assignment tasks, supporting distributed collaboration and individual accountability.

### 4.3.6 Relationship Between GitHub Evidence and Lean Software Development

The group's use of GitHub supports several Lean Software Development principles.

| Lean Principle | Application in the Group Assignment | Expected Benefit |
|---|---|---|
| **Eliminate Waste** | Maintain assignment documents in a shared repository instead of distributing multiple separate file versions. | Reduces duplicated work and confusion over document versions. |
| **Build Quality In** | Revise technical explanations, user stories, and process descriptions throughout the assignment. | Helps identify and correct documentation problems earlier. |
| **Create Knowledge** | Share assignment documents and maintain a visible revision history. | Improves knowledge sharing among group members. |
| **Deliver Fast** | Submit smaller documentation updates instead of waiting until the entire assignment is completed. | Supports earlier feedback and continuous progress. |
| **Respect People** | Allow individual members to contribute through their respective GitHub accounts. | Encourages individual responsibility and collaboration. |

### 4.3.7 Relationship Between GitHub Evidence and XP Practices

The group's GitHub workflow supports the collaborative and iterative principles of Extreme Programming.

| XP Practice | Relationship to the Group's GitHub Workflow |
|---|---|
| **Pair Programming** | The shared repository can support collaborative development and review. However, commit history alone does not prove that pair programming sessions occurred. |
| **Test-First Programming (TDD)** | GitHub can record automated test cases and implementation changes. The current documentation commits do not independently demonstrate that TDD was performed. |
| **Continuous Integration (CI)** | Frequent commits support the principle of integrating small changes regularly. Automated CI execution would require separate evidence, such as GitHub Actions logs and test results. |

The repository demonstrates iterative documentation and distributed contributions. The proposed XP engineering practices for GrabCampus are described separately in Section 4.2.


### 4.3.8 Evidence Verification Summary

| Assignment Evidence Requirement | Verification Status |
|---|---|
| Contributions from all five group members | Confirmed from the screenshots |
| Individual GitHub contributor accounts | Confirmed from the screenshots |
| Meaningful assignment documentation and revisions | Supported by visible commit messages |
| Contributions across multiple weeks | Screenshots show 17–24 November 2026; confirm the complete period using the original Git log |
| Individual USM student email addresses | Pending verification through the original Git log |
| Unedited GitHub commit-history screenshots | Provided in Figures 1 and 2 |
| Original textual Git log | Pending attachment |

Overall, the GitHub repository provides evidence of collaboration, iterative documentation, and distributed contributions among all five group members. The original Git log should also be attached to complete the mandatory evidence requirements specified in the assignment brief.

---

## 4.4 Conclusion

The proposed Lean Waste Audit identifies four potential sources of inefficiency in the GrabCampus booking and payment workflow: Defects and Rework, Waiting, Extra Features, and Extra Processing.

These wastes may affect software quality, development time, and collaboration between team members.

To address these risks, three Extreme Programming practices are recommended.

Pair Programming supports collaborative problem-solving and early defect detection. Test-Driven Development helps developers verify booking and payment requirements before implementation is considered complete. Continuous Integration automates repetitive checks and provides faster feedback on code changes.

These practices also complement Task 1's code review and static analysis process and Task 2's Agile User Stories and GWT acceptance criteria.

Overall, the proposed improvements aim to reduce unnecessary development effort, improve code quality, and support a more efficient software development process.

Their effectiveness should be evaluated using actual project evidence, including automated test results, pull-request records, task completion times, and GitHub commit history.

---

## References


