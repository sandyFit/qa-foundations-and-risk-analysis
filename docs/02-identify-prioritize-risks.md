# Part 2: Identify and Prioritize Risks

1. **Bad pricing calculation in checkout:**
    1. Incorrect tax calculation
    2. Mismatches between prices in the product details and the checkout.
    3. Double pricing
    4. Incorrec total pricing
    
    **Impact**: Revenue loss, refunds, chargebacks, bad reputation, compliant issues, legal risks
    
    **Priority:** Citical because it directly affects the business revenue and reputation.
---
    
2. **Checkout Authorization Failure:** Guest users can access the checkout page without logging in.
    
    Impact: Could leak customers data (PII) or bypass payment validation.
    
    Priority: Critical because it compromises site security.
--- 

3. **Items added to the cart are not shown in the checkout**: The user adds a product to the cart from the product details page, the add to cart button changes to “remove” and the cart badge in the header displays a new item was added but the product doesn’t show in the cart at checkout.
    
    **Impact**: Sales loss, abandoned carts.
    
    **Priority**: Critical because it affects  business revenue and customer satisfaction.
---     
4. **Invalid checkout Inputs:** Validated orders may contain incorrect shipping data.
    
    **Impact**: Merchandise loss or extra shipping costs.
    
    **Priority**: High because it may affect shipping logistic, affecting costs and customer satisfaction.
---

5. **Bad cart badge count:** Removing or adding a new item to the cart doesn’t reflect what the badge cart displays.
    
    **Impact**: Bad cusstomer experience.
    
    **Priority**: Medium because it may cause customer confusion but not direct revenue loss, the user can still finish the checkout process.
