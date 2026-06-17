# Test Summary — Báo cáo tổng hợp kiểm thử

---

## 1. Group info

| Mục | Thông tin |
|-----|----------|
| **Group** | Group 33 |
| **Class** | 252ICT2012.P1 |
| **Date** | 17/6/2026 |
| **Test system** | https://stqa.rbc.vn — v1.0 |

---

## 2. Overall results

| Metric | Value |
|--------|---------|
| Total TCs | 50 |
| Pass | 37 |
| Fail | 13 |
| Blocked | 0 |
| Not Run | 0 |
| **Pass rate** | 74.00% |
| **Bugs spotted** | 10 |

### Results by Functional Group

| Group | Total TC | Passed | Failed | Passed Rate |
|------|---------|------|------|------------|
| REQ-01: Login | 9 | 5 | 4 | 55.56% |
| REQ-02: View book list | 8 | 8 | 0 | 100.00% |
| REQ-03: Search and filter books | 10 | 8 | 2 | 80.00% |
| REQ-04: Borrow Book | 6 | 4 | 2 | 66.67% |
| REQ-05: Return Book | 6 | 5 | 1 | 83.33% |
| REQ-06: Overdue Handling | 2 | 1 | 1 | 50.00% |
| REQ-07: Member management | 6 | 2 | 4 | 66.67% |
| REQ-08: Borrow record lookup | 3 | 2 | 1 | 66.67% |

### Bugs by severity

| Severity | Number | Bug IDs |
|--------|---------|---------|
| High | 6 | BUG-04, BUG-05, BUG-06, BUG-07, BUG-09, BUG-10|
| Medium | 3 | BUG-01, BUG-02, BUG-03|
| Low | 1 | BUG-08 |

---

## 3. Design techniques applied (IDM / test design)

| Technique | Applied to REQs | # of TCs (approx) | Notes |
|-----------|------------------|------------------:|-------|
| EP | REQ-01, REQ-02, REQ-03, REQ-04, REQ-05, REQ-07, REQ-08 | 35 | Widely used for validating valid and invalid input domains and data partitions. |
| BVA | REQ-01, REQ-02, REQ-03, REQ-04 | 4 | Applied to input string lengths and numeric rule limits. |
| Decision table | REQ-01, REQ-03, REQ-04, REQ-07 | 8 | Used for complex logical combinations of business constraints and user roles. |
| STT | REQ-06 | 2 | Used to map and test loan state changes (e.g., Active Loan → Overdue status). |
| Blackbox testing | REQ-01 | 1 | Applied for visual validation and layout behavior of the client application interfaces. |

---

## 4. Quality analysis

### 4.1 Strengths
* REQ-02, REQ-06 and REQ-08 functions well with an approximately 100% pass rate. The system reliably displays complete book lists and processes historical log details without errors.
* Basic keyword search features work well when exact matches are provided, book searching is case-insensitive.

### 4.2 Weaknesses
* Too many issues regarding login and adding newmember.
* System display incorrect error message too often (login, member status, overdued book status, add new member,...)
* Failure to enforce key business rules: the search and filter combined feature executes inconsistently, ignoring keyword query strings or category entirely (BUG-02, BUG-03). The platform allow bypassing the maximum borrowed books threshold (BUG-04).


---
## 5. Bug fixing priority
| Priority | Bug ID | Severity | Rationale |
|---------:|--------|---------:|-----------|
| **1** | BUG-10 | High | Critical privacy breach. Allowing other members to look through one's borrow record is never good for business. |
| **2** | BUG-04 | High | Violates business logic. Bypasses core check-out constraints by allowing a member to borrow a 4th book, risking library stock control. |
| **3** | BUG-07 | High | Violates functional requirements. This allows invalidated member accounts to be created, prone to be attacked.  |
| **4** | BUG-05 | High | Status mapping failure. Incorrectly flags a "Suspended" member as "Expired" -> confusion and administrative mishandling. |
| **5** | BUG-06 | High | No financial punishment, no fine, no nothing. Suppresses penalty warnings during late returns on the interface, creating transparency issues and operational friction. |
| **6** | BUG-09 | High | Violation in functional requirements. Misleads librarians into troubleshooting syntax errors instead of notifying them of an existing duplicated account profile.
| **7** | BUG-01 | Medium | Causes redundant backend data queries on malformed user strings and provides generic, inaccurate feedback for login failures. |
| **8 & 9** | BUG-02 & BUG-03 | Medium | Same priority level. Unreliable search results. Data filtering fails depending on the sequence in which fields are filled, negatively affecting the core lookup module. |
| **10** | BUG-08 | Low | Minor UX issue. Fixing priority: low |



## 6. Conclusion

* System is not ready to be released. There are too many high risk bugs relating to privacy problems and business logic.

---



## 8. AI usage

| AI tools | Used on | How I used |
|------------|-------------------|-----------------------------------|
| chatGPT | `test-cases.md` | I only asked it to make the list of test cases |
