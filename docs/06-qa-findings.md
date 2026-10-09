# QA Test Execution & Release Recommendation Summary

* **Application:** [SauceDemo (Swag Labs)](https://www.saucedemo.com/)

---

## Findings Summary & Insights

* The main happy-path flows (login, adding items, and order completion) are functional.
* Protected routes failures and lack of input sanitization may introduce data corruption, security risks and user experience issues.


---

## 1. What Was Tested

* **Authentication & Session Routing:** Standard user authentication, locked-out account handling, and direct URL route access.
* **Shopping Cart Mutability:** Adding/removing items dynamically, cart badge counter accuracy, and the persistance of items in the cart during page reloading.
* **Checkout Funnel (UI Flow):** Navigation from cart to checkout steps, calculation of subtotals and taxes, and order finalization.
* **Input Validation & Security Constraints:** Long character length injections, HTML/JS script strings, and special characters handling.
* **Exploratory Testing:** Exploratory session covering route navigation, input boundary limits, UI button state toggles, cart mutability and UX worflows.

---

## 2. What Was NOT Tested (Out of Scope / Risk Exposure)

* **Backend API & DB Data Persistence:** Automated API contract validation and direct database schema assertion (tested strictly via frontend UI observation). THe aplication persists data via a javascript.
* **Payment Gateway Integration:** Real credit card processor APIs or third-party webhooks.
* **Cross-Browser / Mobile Responsiveness:** Safari/iOS browsers and mobile environments.
* **Performance & Load Stress:** Heavy concurrent user volume or network latency spikes.

---

## 3. Identified Defects & Technical Concerns

| ID | Type | Description | Technical / Business Impact | Severity / Priority |
| --- | --- | --- | --- | --- |
| **BUG-001** | **UX / Defect** | Form field data clears on browser back navigation during checkout. | User frustration; potential cart abandonment. | `Minor / P3` |
| **INV-001** | **Security / Risk** | Checkout form accepts long string lengths ($>500$ chars) and HTML `<script>` tags. | Risk of `500` server errors, or XSS payload ingestion. | `Major / P2` |
| **INV-002** | **Route Guard / Risk** | Direct navigation to `/checkout-step-two.html` bypasses Step One shipping form completely. | Permits order submission with empty customer shipping info. | `Major / P2` |

---

## 4. Remaining Risks

1. **Database Schema Contamination:** Accepting unvalidated inputs in frontend forms risks passing corrupted user data.
2. **Order Completion Failure:** Bypassing Step One allows orders with default placeholders (*"Free Pony Express Delivery!"*) but no delivery address, requiring manual customer support intervention.

---

## 5. Recommendations

1. **Implement Protected Routes:**
Enforce router checks on `/checkout-step-two.html` so users without active shipping state are redirected to `/checkout-step-one.html` with an explicit error notification.

2. **Apply Basic String Sanitization & Length Boundaries:**
Restrict First Name, Last Name, and Postal Code fields to reasonable max lengths ($Max Length = 50$) and avoid HTML special characters.

3. **Log BUG-001 to Backlog:**
Log BUG-001 as a low-priority quality-of-life ticket in the product backlog. The issue causes non-fatal UX friction during browser back navigation without compromising order fulfillment or security.



---
