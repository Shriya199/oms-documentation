---
description: >-
  Learn how HotWax handles customer email notifications for BOPIS orders in New Era Caps, including order ready for pick-up, cancellations, 
  and planned reminder emails for uncollected orders.
---
# List of Emails Triggered from HotWax

## Email to CS Team

### BOPIS Orders Not Picked Up Email
This email includes orders for all the stores that are not picked up yet by the customer. 
This email is triggered every day, including orders that are in packed status for more than 30 days.

### BOPIS Order Rejection
This email includes orders that are rejected from stores. Orders are rejected for reasons like damaged goods, out-of-stock items, or other fulfillment issues.  
This email is triggered every day for orders rejected in the last 24 hours.

---

## Email to Stores

### BOPIS Orders Not Picked Up Email
This email includes orders that are not picked up in a particular store yet by the customer.  
This email is triggered every day, including orders that are in packed status for more than 30 days.

---

## Email to Customers

### BOPIS Ready for Pick-Up Email
When the order is marked “Ready for pick up”, it’s moved to packed status and shown in the Packed tab of HotWax BOPIS app.  
As soon as the order is marked “Ready for pick up”, an email trigger request is sent to Klaviyo.  
Email is sent from Klaviyo to customers.

### BOPIS Cancellation
When the order/order item is rejected from the HotWax BOPIS app.  
As soon as the order is rejected from the store, an email trigger request is sent from HotWax to Klaviyo.  
Email is sent from Klaviyo to the customer.

### Auto-BOPIS Ready for Pickup Reminder 
When the order is marked “Ready for pick up”, it’s moved to packed status and shown in the Packed tab of HotWax BOPIS app.  
When an order is in packed status for more than 7 days, a reminder pick-up email should be sent to the customer.  
The email trigger request should be sent to Klaviyo every 7th, 14th, and 21st day.  
Email should be sent from Klaviyo to the customer.
