# Task 5: Process Tailoring and Human Factors Governance
**Target Organization and Case Study:** Grab Malaysia — GrabCampus Integrated Booking and GrabPay Checkout

---

## 5.1 Process Tailoring Analysis Using Münch's 4 Ps

No single software methodology can handle both fast consumer app updates and strict payment security at the same time. Following Grab's shift toward early testing, clear Definition of Ready and Definition of Done standards, and automated test layers, we apply Münch’s 4 Ps Process Tailoring Framework covering Problem, Project, People, and Product. This explains why a Hybrid Scrum and V-Model approach fits the GrabCampus Booking and GrabPay Checkout feature best.

### 4 Ps Contextual Audit and Tailoring Matrix

| Münch's Dimension | Audited Organizational and Case Context | Impact on Methodology and Tailoring Justification |
| :--- | :--- | :--- |
| **1. Problem** | **Grab Context:** Grab previously faced slow manual QA checks, high production bug rates, and informal requirement changes from merchant or coordinator chats.<br>**GrabCampus Risk:** Simultaneous sports court or shuttle bookings can cause double bookings, and GrabPay wallet deductions can fail at edge balances like RM 9.99 against a RM 10.00 fee. | Pure Agile lacks formal checks to prevent concurrency and payment bugs, while strict Waterfall is too slow for a fast superapp market. **Tailoring:** Use 2-week Scrum sprints for UI and promo updates alongside V-Model verification gates and early Given-When-Then checks from Task 2 before merging code. |
| **2. Project** | **Grab Context:** Grab runs a continuous superapp ecosystem across 428 cities in 8 countries where release delays hurt competitiveness, yet payment errors break user trust.<br>**USM Audit Context:** Our 5-member student team is running a structured process audit across GitHub, Notion, and WhatsApp under strict commit and consensus rules. | Tight deadlines demand fast feedback without heavy paperwork. **Tailoring:** Enforce Sprint Definition of Ready rules using INVEST stories and Given-When-Then scenarios, plus automated CI/CD gates from Task 1 and Task 4 so V-Model checks run automatically on every Pull Request. |
| **3. People** | **Grab Context:** Cross-functional squads including Product Managers, Tech Leads, Software Engineers, QA Automation Engineers, and DevOps Leads share quality responsibility instead of leaving testing only to QA.<br>**USM Team:** Five Computer Science undergraduates dividing modeling, requirements, testing, Lean, and governance tasks. | Since developers run unit and acceptance tests during coding, clear roles and peer checks are essential. **Tailoring:** Combine Task 1's RACI matrix and Senior Peer Code Reviewer approvals with Task 4's Pair Programming on critical slot locking logic. |
| **4. Product** | **Grab Context:** A critical mobile superapp handling real-time slot states from Available to Locked for 3 minutes and then Booked, alongside high-volume GrabPay transactions.<br>**Audit Deliverable:** A complete GitHub repository, Notion process guide, and synchronized 5-task report. | High-reliability transaction apps need unit, UI component, backend integration, and end-to-end testing. **Tailoring:** Use Scrum for consumer booking screens while mandating V-Model verification and validation from Task 3 for GrabPay APIs, database slot locking, and performance targets. |

### Justification of the Hybrid Approach
- **Why Pure Waterfall Fails:** Waiting until the end of development for manual QA sign-off increases the cost of fixing late defects from Task 3 and creates Lean waiting and extra processing wastes from Task 4. Long linear freeze cycles would slow down Grab's UI promotions and user updates.
- **Why Pure Agile Fails:** Running Agile sprints without strict entry and exit quality gates leaves GrabPay checkouts and 3-minute slot locks vulnerable to race conditions, wrong payment states, and security bugs.
- **How the Hybrid Scrum and V-Model Succeeds:**
  1. **Agile Front-End Sprints:** Consumer UI changes and campus promo features run in 2-week Scrum sprints guided by INVEST user stories and Given-When-Then acceptance criteria from Task 2.
  2. **Automated V-Model Gates:** Critical backend services like GrabPay wallet deduction and atomic 3-minute slot locking follow Task 3’s V-Model mapping, enforced automatically through Task 1’s static analysis gate and Task 4’s TDD and GitHub Actions CI pipeline before deployment.

---

## 5.2 Team Dynamics Evaluation Using Tuckman's Model and Conflict Management

### Current Stage Assessment: Norming, Transitioning into Performing
Using Tuckman’s Stages of Group Development across Forming, Storming, Norming, Performing, and Adjourning, our 5-member CKS222 team currently sits at the Norming stage and is moving into Performing:

1. **Forming:** Team members set up our WhatsApp group, created the shared GitHub repository and Notion workspace, and divided Tasks 1 to 5. Discussions were polite and focused on understanding the assignment boundaries.
2. **Storming:** Some disagreement appeared over how strict our tailored Grab process should be, specifically whether to focus purely on Agile speed or keep formal V-Model verification gates, and how to keep formatting consistent across GitHub sections.
3. **Norming:** The team agreed on clear shared standards by anchoring all five tasks to the GrabCampus Booking and GrabPay Checkout case, linking Task 1 RACI roles with Task 2 and Task 4 test cases, and using Fist-of-Five voting to settle process decisions.
4. **Performing:** With individual sections reviewed and synced on GitHub, the group is shifting focus to combining the final report and preparing each other for the Viva Voce oral defense.
5. **Adjourning:** After submitting the assignment, signing the Group Contribution Log, and completing the Week 9 Viva oral defense, the team will conduct a brief wrap-up review and archive our GitHub and Notion deliverables.

### Conflict Management Analysis
- **Identified Conflict:** During Task 5 planning, one teammate pointed out that adding formal V-Model verification gates could slow down Grab’s 2-week sprint releases against competitors, matching Task 4's warning about extra processing waste.
- **Resolution Approach:** Instead of forcing a majority vote, the Task 5 lead used Fist-of-Five facilitation to address the concern directly. The team agreed to apply strict V-Model gates only to critical payment and concurrency modules while automating static scans and regression tests inside the CI/CD pipeline, linking Tasks 1, 3, and 4 to keep sprints fast and payments safe.

---

## 5.3 Fist-of-Five Consensus Governance and Live Chat Evidence

To finalize our Hybrid Scrum and V-Model Tailoring Strategy, all 5 group members held a live Fist-of-Five consensus vote in our WhatsApp group chat, where 0 means a full block, 1 or 2 signals concerns that need discussion, 3 is neutral, and 4 or 5 shows strong support.

### Voting Outcome and Resolution Summary
- **Session Flow:** The Task 5 lead started the vote by proposing the Hybrid Scrum and V-Model plan. In the first round, three members immediately voted 5 fingers in full support, while one member voted 2 fingers because she worried formal verification gates might slow down sprint releases. After the team agreed to automate those checks inside the CI/CD pipeline to protect sprint speed, she updated her vote to 5 fingers, giving the group unanimous support.
- **Initial Consensus:** Most members agreed right away with combining 2-week Scrum sprints for UI and promo updates with formal V-Model gates for GrabPay and backend booking services, casting 5-finger votes.
- **Concern Raised:** One teammate voted 2 fingers, noting that formal V-Model gates could delay Grab's sprint releases when competing against market rivals.
- **Resolution and Final Agreement:** By limiting strict V-Model gates to high-risk payment and security modules and automating static analysis and regression checks in CI/CD alongside Task 1 and Task 4, the concern was resolved and the teammate changed her vote to 5 fingers.

### Unedited WhatsApp Group Chat Evidence

<img width="1600" height="899" alt="WhatsApp Image 2026-11-17 at 14 50 37" src="https://github.com/user-attachments/assets/ee48448b-1187-4ffc-9904-19d25d9ab7ce" />

<img width="1600" height="899" alt="WhatsApp Image 2026-11-17 at 14 54 52" src="https://github.com/user-attachments/assets/ede8923a-7302-41df-a578-3dbc2d9d335a" />

<img width="1600" height="899" alt="WhatsApp Image 2026-11-17 at 14 55 17" src="https://github.com/user-attachments/assets/58a2d605-d4b5-4824-afbc-c5409e76c577" />

<img width="1600" height="899" alt="WhatsApp Image 2026-11-17 at 14 56 38" src="https://github.com/user-attachments/assets/60d9b34f-42e5-4946-845f-a3246a9e9b57" />
