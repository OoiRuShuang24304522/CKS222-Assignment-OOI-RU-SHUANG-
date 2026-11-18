# Task 5: Process Tailoring (Münch's 4 Ps) & Human Factors Governance
**Target Organization & Case Study:** Grab Malaysia — GrabCampus Integrated Booking & GrabPay Checkout

---

## 5.1 Process Tailoring Analysis Using Münch's 4 Ps

No single off-the-shelf software methodology satisfies both rapid consumer superapp innovation and zero-tolerance financial transaction security. Drawing on Grab’s engineering transition toward **shift-left testing**—incorporating Definition of Ready (DoR), Definition of Done (DoD), and a balanced multi-layer testing strategy—we apply **Münch’s 4 Ps Process Tailoring Framework** (Problem, Project, People, Product) to justify a **Hybrid Scrum + V-Model** lifecycle for the **GrabCampus Booking & GrabPay Checkout** feature.

### 4 Ps Contextual Audit & Tailoring Matrix

| Münch's Dimension | Audited Organizational & Case Context (Grab Malaysia & USM Team) | Impact on Methodology & Tailoring Justification |
| :--- | :--- | :--- |
| **1. Problem** | **Grab Context:** Historically suffered from late-stage manual QA bottlenecks and high production defect leakage (peaking in 2019), alongside informal requirement creep (e.g., ad-hoc campus coordinator or merchant requests via WhatsApp).<br>**GrabCampus Risk:** Race conditions in simultaneous sports court/shuttle reservations (double booking) and inconsistent GrabPay wallet deductions (e.g., RM 9.99 boundary balance vs. RM 10.00 fee). | Pure ad-hoc Agile lacks formal verification gates to prevent concurrency and payment bugs, whereas rigid Waterfall cannot adapt to shifting superapp market needs. **Tailoring:** Combine **2-week Scrum sprints** for iterative UI/promo delivery with **V-Model symmetrical verification/validation gates** and shift-left GWT checks (Task 2) before merge. |
| **2. Project** | **Grab Context:** Continuous, multi-release superapp ecosystem spanning 428 cities across 8 countries where release delays hurt market competitiveness, yet downtime or payment errors erode user trust.<br>**USM Audit Context:** 5-member distributed student team executing a structured process audit across GitHub, Notion EPG, and WhatsApp under strict anti-AI commit and consensus gates. | Tight delivery timelines require fast feedback loops without bureaucratic documentation overhead. **Tailoring:** Enforce **Sprint DoR** (complete INVEST stories + GWT scenarios) and **Automated CI/CD Gates** (Task 1 SonarQube static scans + Task 4 GitHub Actions TDD suites) so V-Model checks execute automatically on every Pull Request. |
| **3. People** | **Grab Context:** Cross-functional squads (Product Manager, Tech Lead/Architect, Software Engineer/Backend Microservice Developer, QA Automation Engineer, DevOps/DevSecOps Lead) transitioning from siloed manual QA to shared quality ownership.<br>**USM Team:** 5 Computer Science undergraduates dividing specialized modeling, requirements, V&V, Lean/XP, and governance tasks asynchronously. | Because software engineers execute acceptance and unit tests directly during development (shift-left), clear role accountability and collaborative review are mandatory. **Tailoring:** Pair Task 1's **RACI Matrix** and **Senior Peer Code Reviewer** sign-offs with Task 4's **XP Pair Programming** on high-risk concurrency lock logic. |
| **4. Product** | **Grab Context:** Mission-critical, multi-service mobile superapp integrating real-time availability state machines (`Available` → `Locked` [3-min TTL] → `Booked`) with high-concurrency GrabPay financial transactions.<br>**Audit Deliverable:** Integrated GitHub repository, Notion SPEM 2.0 EPG, and synchronized 5-task engineering report. | High-reliability transactional products require multi-layer verification (Unit, UI Component with ≥70% coverage, Backend Integration, and E2E testing). **Tailoring:** Use **Agile/Scrum** for consumer-facing booking UI iteration while mandating **symmetrical V-Model V&V** (Task 3) for GrabPay API integration, Redis/DB slot-locking, and non-functional SLAs. |

### Justification of the Hybrid Prescriptive Model (Why Not Pure Waterfall or Pure Agile?)
* **Why Pure Linear Waterfall Fails:** Waiting until the end of the SDLC for manual QA sign-off amplifies the **Defect Multiplier** (Task 3) and generates Lean **Waiting** and **Extra Processing** wastes (Task 4). In Grab’s competitive Southeast Asian ecosystem, multi-month linear freeze cycles delay UI promotions and user experience improvements.
* **Why Pure Ad-Hoc Agile Fails:** Unconstrained Agile iteration without formal entry/exit verification gates exposes GrabPay checkout flows and 3-minute concurrency locks to race conditions, partial payment states, and security vulnerabilities.
* **How the Tailored Hybrid Scrum + V-Model Succeeds:**
  1. **Agile Front-End & Sprint Cadence:** Consumer UI updates and campus promo cards run in **2-week Scrum sprints** governed by INVEST user stories and GWT acceptance criteria (Task 2).
  2. **Embedded V-Model Gates via CI/CD:** Critical backend microservices (GrabPay wallet deduction and atomic 3-minute slot locking) follow **Task 3’s symmetrical V-Model mapping**, enforced automatically through **Task 1’s Static Analysis Gate** and **Task 4’s TDD + GitHub Actions CI pipeline** prior to staging deployment.

---

## 5.2 Team Dynamics Evaluation (Tuckman's Model) & Conflict Management

### Current Stage Assessment: **Norming (Transitioning into Performing)**
Applying **Tuckman’s Stages of Group Development** (Forming, Storming, Norming, Performing), our 5-member CKS222 team currently operates at the **Norming** stage and is transitioning into **Performing**:

1. **Forming:** Members established our WhatsApp coordination group, set up the shared GitHub repository and Notion EPG workspace, and partitioned Tasks 1–5. Communication was polite and exploratory as members clarified assignment scope boundaries.
2. **Storming:** Constructive friction surfaced around cross-task consistency and process strictness—specifically whether our tailored Grab process should prioritize pure Agile speed or retain formal V-Model verification gates, and how to align technical artifacts across separate GitHub sections.
3. **Norming (Current Stage):** The group established explicit shared norms and interfaces: anchoring all five tasks to a unified case scenario (GrabCampus Integrated Booking & GrabPay Checkout), cross-referencing RACI roles (Task 1) with GWT/TDD test cases (Tasks 2 & 4), and resolving methodological disagreements through **Fist-of-Five consensus voting**.
4. **Performing:** With individual task modules peer-reviewed and synchronized in GitHub, the team is shifting focus to seamless report integration, final PDF formatting, and mutual preparation for the Viva Voce defense.

### Conflict Management Analysis
* **Identified Conflict:** During Task 5 tailoring, a teammate raised a valid concern that adding formal V-Model verification gates could slow down Grab’s 2-week sprint release velocity against market rivals, echoing Task 4's warning against Extra Processing waste.
* **Resolution Approach (Collaborative Compromise):** Instead of forcing a majority override, the Task 5 lead used structured **Fist-of-Five facilitation** to surface the exact bottleneck. By scoping V-Model verification gates strictly to critical payment and concurrency modules and automating static analysis and regression checks inside the CI/CD pipeline (linking Tasks 1, 3, and 4), the team preserved sprint velocity while guaranteeing payment safety.

---

## 5.3 Fist-of-Five Consensus Governance & Live Chat Evidence

To ratify our **Hybrid Scrum + V-Model Tailoring Strategy**, all 5 group members participated in a live **Fist-of-Five consensus session** in our WhatsApp group chat (`0` = Total Block, `1` = Major Concerns, `2` = Minor Concerns/Needs Discussion, `3` = Neutral/Acceptable, `4` = Good Idea, `5` = Full Support).

### Voting Outcome & Resolution Summary
* **Initial Consensus:** Following the proposal to combine 2-week Scrum sprints for consumer UI/promo updates with formal V-Model verification gates for GrabPay and backend booking updates, the majority of the team immediately agreed and cast **5-finger votes**.
* **Concern Raised (Vote Below 3):** One teammate voted **2 fingers**, raising the concern that introducing formal V-Model verification gates might slow down Grab's sprint releases when competing against market rivals.
* **Resolution & Final Agreement:** To resolve this concern without sacrificing payment reliability, the team agreed to scope strict V-Model gates exclusively to critical payment and security modules while automating static analysis and regression checks inside the CI/CD pipeline (aligning with Task 1 and Task 4). Satisfied that sprint speed would remain intact, the teammate updated their vote to **5 fingers**, achieving unanimous consensus across all 5 members.

### Unedited WhatsApp Group Chat Evidence (Fist-of-Five Gate)

<img width="1600" height="899" alt="WhatsApp Image 2026-11-17 at 14 50 37" src="https://github.com/user-attachments/assets/ee48448b-1187-4ffc-9904-19d25d9ab7ce" />

<img width="1600" height="899" alt="WhatsApp Image 2026-11-17 at 14 54 52" src="https://github.com/user-attachments/assets/ede8923a-7302-41df-a578-3dbc2d9d335a" />

<img width="1600" height="899" alt="WhatsApp Image 2026-11-17 at 14 55 17" src="https://github.com/user-attachments/assets/58a2d605-d4b5-4824-afbc-c5409e76c577" />

<img width="1600" height="899" alt="WhatsApp Image 2026-11-17 at 14 56 38" src="https://github.com/user-attachments/assets/60d9b34f-42e5-4946-845f-a3246a9e9b57" />
