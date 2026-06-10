# Signal – Private Messenger | Bug Report

## About the Project

Signal is a free, open-source, end-to-end encrypted messaging app used by millions worldwide.
It is developed by the Signal Foundation and is widely regarded as one of the most secure
private messaging platforms available. Beyond messaging, Signal supports voice/video calls,
disappearing messages, and a built-in donation feature that allows users to financially
support the Signal Foundation directly from the app.

This testing was performed on the **Android version (v8.7.3)** of the Signal app, focusing
on the **Donation module** — specifically the custom amount input field. The goal was to
evaluate input validation behaviour and boundary value handling in a real-world financial
workflow on a mobile application.

**Testing was performed for educational and portfolio purposes only.**

---

## BUG-008 — Donation Custom Amount Field Accepts Unrealistically Large Values — No Input Validation

| Field | Details |
|---|---|
| App | Signal – Private Messenger |
| App Version | 8.7.3 |
| Device | Samsung Galaxy A22 4G |
| OS | Android 13 |
| Severity | 🔴 High |
| Priority | 🔴 High |
| Type | Input Validation / Boundary Value Failure |
| Module | Donation Screen → Custom Amount Field |
| Status | Open |
| GitHub Issue | [#14750](https://github.com/signalapp/Signal-Android/issues/14750) |
| Date Reported | 1 May 2026 |
| Test Type | Manual / Exploratory |

---

## Bug Summary

The custom donation amount field has no upper limit validation. Users can enter a number
with 30+ digits and the app allows them to proceed to the payment screen without any error
or warning.

---

## Steps to Reproduce

1. Open Signal app
2. Tap Profile icon → **Donate to Signal**
3. On the Donation screen, tap the **custom amount input field**
4. Type a very large number e.g. `9999999999999999999999999`
5. Observe — no error shown, Continue button stays active
6. Tap **Continue**
7. Observe — app navigates to payment screen successfully

---

## Expected Result

- App should enforce a maximum donation limit
- Inline error should appear: *"Please enter an amount between $1 and $1,000"*
- **Continue** button should be disabled for invalid amounts
- User should NOT be able to reach the payment screen

---

## Actual Result

- App accepts any number regardless of size (30+ digits)
- No validation error or warning is shown
- **Continue** button remains active
- User reaches the payment screen with an invalid amount

---

## Impact

A user (or malicious actor) can submit an unrealistically large donation amount and reach
the payment processing screen. Depending on backend handling, this could cause transaction
errors, data inconsistencies, or unexpected application behaviour.

---

## Evidence

👉 [View on GitHub Issue #14750](https://github.com/signalapp/Signal-Android/issues/14750)

---

## 🧠 Testing Approach

- **Exploratory Testing** — Navigated through the app without a fixed script to discover
  unexpected behaviour in less-obvious modules like the donation flow
- **Boundary Value Analysis (BVA)** — Tested the edges of the input field by entering
  extremely large values to check if any upper limit was enforced
- **Negative Testing** — Intentionally entered invalid data to verify the app handled it
  gracefully with appropriate error messages
- **Mobile UI Testing** — Observed how the app responded visually on a real Android device
  (Samsung Galaxy A22 4G) rather than an emulator
- **Workflow Testing** — Followed the full donation flow end-to-end (amount entry →
  Continue → payment screen) to understand where the breakdown occurred

---

## 📈 Key Learnings

- **Input validation must be enforced at both the UI and backend level.** Relying on
  frontend-only checks is not sufficient — but even the UI check was missing here
- **Financial flows require stricter boundary testing.** Any feature involving money
  (donations, payments, transfers) should be tested with minimum, maximum, zero, negative,
  and extremely large values as standard practice
- **Mobile apps need the same rigour as web apps.** This bug demonstrates that mobile
  application inputs are equally prone to validation gaps and deserve the same structured
  testing approach
- **Exploratory testing uncovers what scripted testing misses.** This bug was found by
  freely exploring the app rather than following a predefined test case, highlighting the
  value of unscripted investigation
- **Open source contribution builds credibility.** Filing a structured, evidence-backed
  issue on a high-profile repository like Signal Android demonstrates professional
  communication and real-world testing skills
