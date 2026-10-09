# Part 1: Understanding the Product

1. The application is an e-commerce for clothing, bags and accesories. It’s primary business is to convert visitors into merchandise customers.

2. Users: Online shoppers looking for appareal merchandise.

3. Most important business workflows:
    1. Authentication and user access: Logging in with provided valid credentials, logging out, access control to inventory and cart.
    2. Inventory exploration: Filter products by name or price, link to product details, add to cart and remove features.
    3. Cart: Items can be added from inventory or product details pages, item removal feature, state verification.
    4. Checkout: Takes user shipping data: name, lastname, postal code. Calculates total items, tax, and total price.

4. Critical features failures:
    1. Checkout calculations failure: Could impact company’s revenue or produce compliant issues (double pricing, incorrect tax calculation)
    2. Authentication: Access to checkout without proper authentication could impact the security of the site or exposure of customers PII.

5. Missing Informations and unclear requirements:
    1. Checkout form inputs: No clear information about allowed characters or length. Doesn’t validate postal codes (accepts special characters and spaces).
    2. Quantities for products can’t be adjusted.
    3. No out of stock information.
    4. No specified timeouts for items in cart.
    
6. Questions:
    1. What is the validation format for the input fields in the checkout form?
    2. How are out-of-stock products managed?
    3. Can users buy more than one item of each product?
    4. Should items persist in the cart after the user logs out?
    5. How abandoned carts are managed?
