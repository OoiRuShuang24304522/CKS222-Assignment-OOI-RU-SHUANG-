# Task 3: Symmetrical Verification & Validation (V-Model Design)
To eliminate late-stage defect leaks in Grab Malaysia’s booking and payment ecosystem, ad-hoc end-of-lifecycle manual testing is replaced with a symmetrical V-Model verification and validation (V&V) architecture. Because the cost of fixing software defects escalates exponentially across the Software Development Life Cycle (SDLC)—known as the Defect Multiplier—the V-Model enforces preventive engineering by pairing every constructive specification phase on the descending left arm with a horizontally aligned analytical testing phase on the ascending right arm. During each left-side design activity, engineers simultaneously author the test specifications for its counterpart right-side testing phase, ensuring full requirements traceability and zero late-stage surprises.

## 3.1 Custom V-Model Symmetrical Mapping: GrabPay Secure Payment & Concurrent Booking Engine
The custom V-Model below governs the development of Grab Malaysia’s GrabPay Secure Payment & Atomic Booking Ledger, a core transactional feature responsible for preventing double-booking race conditions via concurrency locks while guaranteeing atomic GrabPay wallet deductions.



Horizontal Phase-by-Phase Alignment
1. Level 1: Business & User Requirements $\longleftrightarrow$ User Acceptance Testing (UAT)
    
2. Level 2: Software & System Requirements (SRS) $\longleftrightarrow$ System & Non-Functional Testing

3. Level 3: Microservice & API Architectural Design $\longleftrightarrow$ Subsystem & API Integration Testing

4. Level 4: Detailed Module & Database Schema Design $\longleftrightarrow$ Automated Unit & Static Testing

5. Apex: Implementation & Secure Coding
