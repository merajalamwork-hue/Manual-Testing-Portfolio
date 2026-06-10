# 🧪 Manual Testing Portfolio

| Testing Type | Status | Bugs Found | Apps Tested |
|---|---|---|---|
| Security & Business Logic | ✅ Active | 3 | 1 |
| Functional & UI | ✅ Active | 4 | 4 |
| API Testing | ✅ Active | 1 | 1 |

A structured portfolio of bugs discovered through manual exploratory testing across multiple web applications — covering security vulnerabilities, business logic flaws, functional/UI issues, and API error handling.

All testing was performed on authorized or intentionally vulnerable applications for educational and portfolio purposes only.

---

## 👨‍💻 About Me

Manual Software Tester with hands-on experience in functional testing, exploratory testing, input validation, API testing, and basic security testing. This repository documents real bugs I identified, analyzed, and reported across multiple applications.

---

## 📊 Summary

| Metric | Count |
|---|---|
| Applications Tested | 6 |
| Total Bugs Found | 8 |
| Security / Business Logic Bugs | 3 |
| Functional / UI Bugs | 4 |
| API Bugs | 1 |
| Critical / High Severity | 4 |
| Medium Severity | 3 |
| Low Severity | 1 |

---

## 🧪 Applications Tested

| Application | Type | Testing Focus |
|---|---|---|
| VulnBank | Intentionally Vulnerable Banking App | Security & Business Logic |
| Mifos X Web App | Open Source Finance App | UI & Functional |
| WordPress Playground | Sandbox CMS Environment | Functional & Workflow |
| https://app.plane.so/ | Web Application (SaaS) | UI & Functional |
| AutoMQ | Open Source Message Queue | API Testing & Error Validation |
| Signal – Private Messenger | Mobile App (Android) | Input Validation / Functional |

---

## 📁 Repository Structure

```
Manual-Testing-Portfolio/
├── README.md                          # This file — full summary
├── security-testing/
│   └── vulnbank_bugs.md               # Security & business logic bugs
├── functional-testing/
│   ├── mifos_bugs.md                  # UI bug — Mifos X
│   ├── wordpress_bugs.md              # Workflow bug — WordPress
│   ├── plane_bugs.md                  # UI & workflow bugs — Plane.so
│   └── signal_bugs.md                 # Input validation bug — Signal Android
├── api-testing/
│   └── automq-api-testing.md          # API testing — AutoMQ
└── evidence/
    └── *.mp4 / *.png                  # Screenshots and recordings
```

---

## 🔐 Security Testing — VulnBank

Business logic and input validation vulnerabilities found in an intentionally vulnerable banking application.

### 🔴 BUG-001 — Payment Process Bypasses Virtual Card Requirement

| Field | Details |
|---|---|
| Severity | Critical |
| Type | Business Logic Flaw |
| Module | Bill Payment |
| Impact | Unauthorized or invalid transactions possible |

The bill payment flow allows users to complete payments without possessing a valid virtual card, bypassing a core financial security control.

👉 [View Full Issue Report](./security-testing/vulnbank_bugs.md)

---

### 🔴 BUG-002 — Improper Validation of Recipient Account in Fund Transfer

| Field | Details |
|---|---|
| Severity | High |
| Type | Input Validation / Business Logic |
| Module | Fund Transfer |
| Impact | Transaction integrity compromised |

The fund transfer module accepts invalid or non-existent recipient account details without any validation, allowing transactions to proceed to incorrect destinations.

👉 [View Full Issue Report](./security-testing/vulnbank_bugs.md)

---

### 🔴 BUG-003 — Negative Loan Amount Accepted Due to Missing Input Validation

| Field | Details |
|---|---|
| Severity | High |
| Type | Input Validation |
| Module | Loan Application |
| Impact | Financial and logical inconsistency |

The loan amount input field accepts negative values with no server-side or client-side validation, creating a logical flaw that could be exploited to manipulate financial calculations.

👉 [View Full Issue Report](./security-testing/vulnbank_bugs.md)

---

## 🖥️ Functional & UI Testing

### 📁 Mifos X Web App

#### 🟡 BUG-004 — Username Text Overlaps Account Icon on Login Page

| Field | Details |
|---|---|
| Severity | Medium |
| Type | UI Bug |
| Module | Login Page |
| Impact | Readability and user experience degraded |

When a long username is entered in the login page input field, the text overlaps with the account icon inside the input box, making the field difficult to read and visually broken.

👉 [View Evidence](./functional-testing/mifos_bugs.md)

---

### 📁 WordPress Playground

#### 🟠 BUG-005 — 404 Error After Creating New User in Admin Panel

| Field | Details |
|---|---|
| Severity | Medium |
| Type | Functional / Workflow Bug |
| Module | Admin Panel → User Management |
| Impact | Core admin functionality broken |

**Steps to Reproduce:**
1. Open WordPress Playground
2. Navigate to Users → Add New
3. Enter a valid username and email address
4. Click "Create New User"
5. Observe the redirect

**Expected:** User is created and system redirects to user list or confirmation page

**Actual:** Application redirects to a 404 Page Not Found screen, breaking the entire workflow with no success or failure feedback

👉 [Evidence](https://github.com/user-attachments/assets/f685cf31-d0a3-443a-87e1-6fad8016788f)

---

### 🌐 Plane.so

#### 🟡 BUG-006 — "+ Add Quick Link" Text Overlaps Plane AI Text on Mobile Browser

| Field | Details |
|---|---|
| Severity | Medium |
| Type | UI / Layout Bug |
| Module | Home Page → Quicklinks & Plane AI Section |
| Impact | Visual confusion for all mobile users on first screen after login |

**Steps to Reproduce:**
1. Open Chrome browser on a mobile device
2. Navigate to https://app.plane.so
3. Sign in with your account
4. Observe the Home page immediately after login
5. Look at the area between the Plane AI section and the Quicklinks section

**Expected:** "+ Add Quick Link" button and Plane AI text render in clearly separated sections

**Actual:** The two UI elements visually overlap on mobile viewport, merging text from different sections

| Mode | Behaviour |
|---|---|
| Desktop browser | ✅ No overlap — layout renders correctly |
| Mobile Chrome | ❌ Overlap occurs — sections bleed into each other |

Environment: Samsung Galaxy A22 4G · Android 13 · Chrome 148.0.7778.120

👉 [View Issue](./functional-testing/plane_bugs.md)

---

### 📱 Signal – Private Messenger (Android)

#### 🔴 BUG-008 — Donation Custom Amount Field Accepts Unrealistically Large Values — No Input Validation

| Field | Details |
|---|---|
| Severity | High |
| Type | Input Validation / Boundary Value Failure |
| Module | Donation Screen → Custom Amount Field |
| Status | Open |
| GitHub Issue | [#14750](https://github.com/signalapp/Signal-Android/issues/14750) |
| Date Reported | 1 May 2026 |
| Impact | Users can reach payment screen with invalid 30+ digit amounts; no error or limit enforced |

**Steps to Reproduce:**
1. Open Signal app
2. Tap Profile icon → **Donate to Signal**
3. Tap the **custom amount input field**
4. Enter a very large number (e.g. `9999999999999999999999999`)
5. Observe — no error shown, Continue button stays active
6. Tap **Continue**
7. Observe — app navigates to payment screen successfully

**Expected:** App enforces a maximum donation limit; inline error message appears; Continue button is disabled for invalid amounts

**Actual:** App accepts any number regardless of size (30+ digits) with no validation error; user proceeds to payment screen with an invalid amount

Environment: Samsung Galaxy A22 4G · Android 13 · Signal v8.7.3

👉 [View GitHub Issue](https://github.com/signalapp/Signal-Android/issues/14750)

---

## 🌐 API Testing — AutoMQ

API error handling and information disclosure bug found during manual API testing of the AutoMQ open source message queue platform.

### 🟢 BUG-007 — Internal Java Error Details Exposed in API Response for Invalid Parameter

| Field | Details |
|---|---|
| Severity | Low |
| Type | Error Handling / Information Disclosure |
| Module | Broker API — `pageSize` query parameter |
| Status | ✅ Closed — Acknowledged by maintainer |
| GitHub Issue | [#3366](https://github.com/AutoMQ/automq/issues/3366) |
| Date Reported | 21 May 2026 |
| Impact | Internal tech stack (Java implementation details) exposed in error response |

Passing an alphabetic string (e.g. `pageSize=abc`) to an integer query parameter caused the API to return a raw Java exception trace instead of a clean validation message. The maintainer confirmed the parameter rejection is by-design due to type mismatch, but acknowledged the error response leaks internal implementation details. Improvement to validation error messages is planned for a future release.

👉 [View Full API Testing Report](./api-testing/automq-api-testing.md)

---

## 🧠 Testing Approach

- Manual exploratory testing
- Input validation & boundary value testing
- Business logic & transaction workflow testing
- API testing with Postman (parameter validation, error response analysis)
- Basic security checks (authentication, authorization flows)
- UI consistency and responsiveness testing
- Mobile app testing (Android — functional and input validation)

---

## 📈 Key Learnings

- How missing input validation creates security and financial risks
- Identifying business logic flaws that bypass critical controls
- Difference between functional bugs and security vulnerabilities
- Writing structured, evidence-backed bug reports
- How workflow-breaking bugs affect core user journeys
- How to test and document API error handling and information disclosure issues
- How to engage with open source maintainers and confirm findings via GitHub Issues
- Boundary value testing on mobile apps and the risks of uncapped numeric input fields

---

## 👤 Author

**Meraj Alam**

GitHub: [@merajalamwork-hue](https://github.com/merajalamwork-hue)
