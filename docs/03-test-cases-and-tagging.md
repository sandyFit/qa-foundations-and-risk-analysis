# Part 3: Design Manual Test Cases & Tagging Taxonomy

### **Tagging Guidelines**
* **Suite Tags:** `[smoke]`, `[sanity]`, `[regression]`, `[e2e]`
* **Type Tags:** `[positive]`, `[negative]`, `[boundary]`, `[security]`, `[data-integrity]`
* **Domain Tags:** `[authentication]`, `[inventory]`, `[cart]`, `[checkout]`

### **Severity & Priority Guidelines**

* **Severity (Impact):** How seriously does a confirmed defect affect system functionality, data integrity, security, or user experience? Severity is based on the defect's impact. (`Blocker`, `Major`, `Normal`, `Minor`)

* **Priority (Business Urgency):** How urgently should a confirmed defect be fixed, considering business impact, user needs, risk, and release deadlines? (`P1-Critical`, `P2-High`, `P3-Medium`, `P4-Low`)

* **Unconfirmed Issues:** Use `TBD` when the impact or urgency cannot yet be assessed. For exploratory testing observations, record the evidence and investigate before classifying the behavior as a defect.

* **Independence:** Severity and priority are related but distinct. A high-severity defect is not automatically the highest priority; business context and risk influence the fix order.

---

## **Test Cases**

### **1. Authentication & Session Management**

#### **TC-AUTH001: Successful Login with Standard User**

**Preconditions:** User is on the login page `https://www.saucedemo.com/`.

**Test Steps:**
  1. Enter `standard_user` into the Username field.
  2. Enter `secret_sauce` into the Password field.
  3. Click the **Login** button.


**Expected Result:**
  * User is successfully authenticated.
  * App redirects to the inventory page (`/inventory.html`).


**Tags:** `[authentication, login, smoke, positive, e2e]`

**Severity:** `Blocker` *(Authentication failure blocks the core application features)*

**Priority:** `P1-Critical`

---

### **TC-AUTH002: Login Attempt with Locked Out Account**

**Preconditions:** User is on the login page `https://www.saucedemo.com/`.

**Test Steps:**
  1. Enter `locked_out_user` into the Username field.
  2. Enter `secret_sauce` into the Password field.
  3. Click the **Login** button.


**Expected Result:**
  * Authentication fails and user remains on the login page.
  * Error message displays: `"Epic sadface: Sorry, this user has been locked out."`


**Tags:** `[authentication, login, negative, security, boundary]`

**Severity:** `Major` *(Improper handling of locked accounts affects access)*

**Priority:** `P2-High`

---

#### **TC-AUTH003: Direct URL Route Bypassing Authentication**

**Preconditions:** User is not authenticated; browser session is logged out or cleared.

**Test Steps:**
  1. Navigate directly to `https://www.saucedemo.com/checkout-step-one.html` via the browser address bar.
  2. Press **Enter**.


**Expected Result:**
  * Route access is blocked.
  * Application redirects to login page or displays error: `"Epic sadface: You can only access '/checkout-step-one.html' when you are logged in."`


**Tags:** `[security, authentication, authorization, routing, boundary]`

**Severity:** `Major` *(This failure allows unauthenticated users to access protected routes)*

**Priority:** `P2-High`

---
### **2. Cart & Inventory State**

#### **TC-CART004: Cart Badge Synchronization when Adding Items**

**Preconditions:** User is logged in as `standard_user` on `/inventory.html` with an empty shopping cart.

**Test Steps:**
  1. Click **Add to cart** for "Sauce Labs Backpack".
  2. Click **Add to cart** for "Sauce Labs Bike Light".


**Expected Result:**
  * Shopping cart header badge updates counter to `2`.
  * Product action buttons toggle state from "Add to cart" to "Remove".


**Tags:** `[cart, inventory, UI-state, smoke, positive]`

**Severity:** `Normal` *(Affects user experience but order still can be placed.)*

**Priority:** `P2-High`

---

#### **TC-CART005: Cart Item Removal & Counter Decay**

**Preconditions:** User is logged in as `standard_user` on `/inventory.html` with 2 items ("Sauce Labs Backpack" and "Sauce Labs Bike Light") in the cart.

**Test Steps:**
  1. Click the shopping cart badge to navigate to `/cart.html`.
  2. Verify two items are present in the checkout list.
  3. Click **Remove** next to "Sauce Labs Backpack".


**Expected Result:**
  * "Sauce Labs Backpack" is removed from the cart list.
  * Shopping cart badge counter updates from `2` to `1`.


**Tags:** `[cart, regression, state-management, positive]`

**Severity:** `Normal` *(Standard cart items counting)*

**Priority:** `P3-Medium`

---

### **3. Inventory & Product Browsing**

#### **TC-INV006: Inventory Sorting by Price (Low to High)**

**Preconditions:** User is logged in and on `/inventory.html`.

**Test Steps:**
  1. Click the product sort filter dropdown in the top right.
  2. Select **Price (low to high)**.


**Expected Result:**
  * Inventory item cards dynamically re-order in strictly ascending numerical order based on price values ($7.99 \rightarrow \$9.99 \rightarrow \dots$).


**Tags:** `[inventory, sorting, regression, data-presentation]`

**Severity:** `Minor` *(UI filtering feature that is non-critical to process an order)*

**Priority:** `P3-Medium`

---

### **4. Checkout & Order Fulfillment**

#### **TC-CKOF007: Checkout Form Submission with Empty Mandatory Fields**

**Preconditions:** User is on `/checkout-step-one.html` with items in the cart.

**Test Steps:**
  1. Leave First Name, Last Name, and Zip/Postal Code inputs completely blank.
  2. Click **Continue**.


**Expected Result:**
  * Form submission is intercepted and blocked; user remains on `/checkout-step-one.html`.
  * Inline validation error displays: `"Error: First Name is required"`.


**Tags:** `[checkout, input-validation, negative, boundary]`

**Severity:** `Major` *(Missing required field validation risks sending incomplete customer data)*

**Priority:** `P2-High`

---

### **TC-CHKF008: Checkout Form Handling of Boundary and Special-Character Input**

**Preconditions:** User is logged in as `standard_user`, has at least one item in the cart, and is on `/checkout-step-one.html`.

**Test Objective:** Evaluate how the checkout form handles unusually long, special-character, and non-standard input values.

**Test Steps:**
  1. Enter a 400+ character string into **First Name**.
  2. Enter `<script>alert('test')</script>` into **Last Name**.
  3. Enter `!@#$%^&*()` into **Zip/Postal Code**.
  4. Click **Continue**.

**Expected Result:**
  * The application handles the supplied input without causing an application error, broken layout, unintended script execution, or unexpected navigation.
  * Any input validation or sanitization behavior defined by the application's requirements should be applied.
  * If no validation requirements are defined for these input classes, the observed behavior should be recorded rather than treated automatically as a defect.

**Observed Result:**
  * Application accepts all supplied values and navigates to `/checkout-step-two.html`.
  * No frontend validation error is displayed.

**Assessment:**
  * **No functional defect can be confirmed solely from this test because maximum field length, allowed character sets, and sanitization requirements are not defined in the available specification.**
  * The behavior may nevertheless represent a **security or input-validation risk** that should be reviewed against the intended product requirements.

**Tags:** `[checkout, security, input-validation, boundary, special-character, exploratory]`

**Severity:** `TBD` — dependent on whether the behavior violates an established security or validation requirement.

**Priority:** `TBD` — should be assigned after confirming the requirement and risk.

---

#### **TC-CHKF-009: Checkout Finalization & Order Completion**

**Preconditions:** User is on `/checkout-step-two.html` with valid cart items and checkout information submitted.

**Test Steps:**
  1. Verify the item summary, price subtotal, tax calculation, and total match expected values.
  2. Click the **Finish** button.


**Expected Result:**
  * App navigates to `/checkout-complete.html`.
  * Confirmation header displays *"Thank you for your order!"* and subheader displays dispatch message.
  * Shopping cart badge is cleared/removed.
  * "Back Home" button navigates cleanly to `/inventory.html`.


**Tags:** `[checkout, smoke, positive, order-fulfillment, e2e]`

**Severity:** `Blocker` *Failure completely prevents the order from being placed.*

**Priority:** `P1-Critical`

---

#### **TC-CHKF-010: Order Cancellation from Summary Overview**

**Preconditions:** User is on `/checkout-step-two.html` with items in the cart.

**Test Steps:**
1. Click the **Cancel** button.


**Expected Result:**
  * User is redirected to `/inventory.html`.
  * Cart state is preserved: items and badge count remain untouched.


**Tags:** `[checkout, regression, navigation, state-preservation]`

**Severity:** `Normal` *(Standard navigation exit route)*

**Priority:** `P3-Medium`

---


