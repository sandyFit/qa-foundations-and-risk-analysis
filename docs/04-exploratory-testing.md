# Exploratory Testing Report — SauceDemo

## Session

* **Target Application:** [SauceDemo (Swag Labs)](https://www.saucedemo.com/)
* **Session Duration:** 45 Minutes
* **Session Strategy:** Session-Based Test Management (SBTM)
* **Tester:** Patricia
* **Primary Focus:** Shopping Cart Mutability, Checkout Navigation State, Boundary Input Ingestion, and Route Guards.

---

## 1. Objectives

> Explore the Shopping Cart and Checkout flows to discover unexpected system behaviors and edge-case validation failures.

### Focus Areas

* **Cart State Mutability:** Adding/removing items repeatedly, multi-tab sync, and price updates.
* **Navigation & Session History:** Browser Back/Forward buttons during multi-step checkout.
* **Order of Execution:** Skipping steps, completing fields out of sequence, and direct URL manipulation.
* **Input Boundary Security:** Unsanitized strings, script tags, extreme lengths, and special characters.

---

## 2. Session Execution & Exploration Log

### Time Block 1: Cart Mutability & Rapid State Changes (00:00 – 00:15)**Actions Taken:**
* Added all 6 inventory items to the cart rapidly from `/inventory.html`.
* Navigated to `/cart.html` and repeatedly removed and re-added items.
* Navigated back to `/inventory.html` and verified button state toggles (`Add to cart` vs. `Remove`).
**Observations:**
* UI state updates synchronously; the badge count accurately reflects item additions and removals.
* Item state remains persistent across page refreshes.



### Time Block 2: Checkout Navigation & State Preservation (00:15 – 00:30)**Actions Taken:**
* Added items, proceeded to `/checkout-step-one.html`, filled out partial data, and clicked the browser **Back** button.
* Proceeded to `/checkout-step-two.html` (Overview) and clicked browser **Back** to Step One.
* Attempted to modify cart contents in another browser tab while on Step Two.
**Observations:**
* Clicking browser **Back** from `/checkout-step-two.html` returns to `/checkout-step-one.html`, but previously entered form fields (First Name, Last Name, Zip) were not preserved**Potential Flaw:** User must re-type all shipping information if they return to verify or modify step one.



---


### Time Block 3: Input Injection & Unexpected Execution Sequences (00:30 – 00:45)

**Actions Taken:**
* Injected large string payloads ($>500$ characters) and HTML/JS strings (`<script>alert('xss')</script>`) into `/checkout-step-one.html`.
* Navigated directly to `/checkout-complete.html` via address bar without completing purchase.


**Observations:**
* **Finding 1:** Form fields accept any string length and raw HTML script tags without frontend sanitization or field length validation.
* **Finding 2:** Direct navigation to `/checkout-step-one.html` while logged out is properly intercepted and redirected to login with an error message.



---

## 3. Summary

| ID | Category | Observed Behavior | Technical Impact | Risk Level |
| --- | --- | --- | --- | --- |
| **OBS-01** | **State Handling** | Form field inputs on `/checkout-step-one.html` reset when navigating backward from Step Two. | Bad UX; forces user re-entry of shipping data. | `Low / Medium` |
| **OBS-02** | **Data Integrity** | Input fields allow infinite character lengths and unsanitized special symbols (`<script>`). | Downstream DB truncation or XSS vulnerability risks. | `High (Security)` |
| **OBS-03** | **Route Guard** | Direct URL access to `/checkout-step-one.html` without logging in is correctly blocked. | Router guards function as expected. | `Pass (Expected)` |





