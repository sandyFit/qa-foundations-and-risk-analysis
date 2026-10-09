# Bug Reports

## **BUG-001: Form Input State Persistence Cleared on Browser Back Navigation During Checkout**

**Bug Title:** Shipping form input fields on `/checkout-step-one.html` reset when returning via browser back button from `/checkout-step-two.html`.

**Environment:**
  * **OS:** Windows 11 Home 
  * **Browser:** Google Chrome (v128.0) 
  * **URL:** [https://www.saucedemo.com/checkout-step-one.html](https://www.saucedemo.com/checkout-step-one.html)


**Preconditions:**
  * User is logged in as `standard_user`.
  * User has at least one item in the shopping cart.



#### **Steps to Reproduce:**

  1. Navigate to the shopping cart (`/cart.html`) and click **Checkout**.
  2. Fill out valid details in `/checkout-step-one.html` (e.g., First Name: `John`, Last Name: `Doe`, Zip: `123456`).
  3. Click **Continue** to navigate to `/checkout-step-two.html` (Checkout: Overview).
  4. Click the browser's **Back** button (or press `Alt + Left Arrow` / `Cmd + [`) to return to Step One.

**Expected Result:**
The browser returns to `/checkout-step-one.html` with all previously entered input values (`John`, `Doe`, `123456`) in the form fields, allowing the user to review without retyping.

**Actual Result:**
The browser returns to `/checkout-step-one.html`, but user info is not preserved, forcing to re-enter all shipping information.

**Severity:** `Minor` (User Experience / Data Handling Failure)

**Observation:** The issue is not a blocker, it does not crash the application or prevent placing the order, but it introduces friction into the checkout funnel and this bad user experience could lead to cart abandonment especialy on slow devices.


---

## Exploratory Notes

### **INV-001: Absence of Input Length Constraints & String Sanitization on Order Form**

* **Category:** Unclear Requirement / Security Risk (Data Integrity & Ingestion)

* **Target Page:** `/checkout-step-one.html` (First Name, Last Name, Zip Code)

#### **Observed Behavior:**

The checkout form accepts long input strings (e.g., 500+ character strings), raw HTML/JS script tags (`<script>alert('test')</script>`), and special characters postal codes (`!@#$%^&*()`). The system navigates to `/checkout-step-two.html` without type validation or length limits.


#### **Questions for Product Manager:**

  * What are the length limits for First Name, Last Name, and Postal Code fields?
  * Should postal code validation be alphanumeric or only accept numbers?

---

### **INV-002: Direct Navigation to Checkout Overview Page (`/checkout-step-two.html`) Skipping Step One**

* **Category:** Suspicious Navigaion Behavior

* **Target Page:** [https://www.saucedemo.com/checkout-step-two.html](https://www.saucedemo.com/checkout-step-two.html)

#### **Observed Behavior:**

Pasting the target page directly into the browser address bar allows authenticated users with items in their cart to bypass `/checkout-step-one.html`. The application renders the `/checkout-step-two.html` page with a default placeholder ("Free Pony Express Delivery!") while omitting required customer shipping details.

#### **Observations:**

  1. **Incomplete Order:** Allowing users to reach the order finalization stage without shipping details allows the user to click **Finish** with empty customer data.
  2. **Route Failure:** Checkout routes should be protected to avoid bypassing mandatory steps for a complete order finalization.

#### **Questions for Product Manager:**

  * Should accessing `/checkout-step-two.html` directly without completing `/checkout-step-one.html` trigger an automatic redirect to `/checkout-step-one.html` or a explicit banner error?

---

