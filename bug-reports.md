# Kerbwise checkout bug reports


**Note on How to start the test** 

Open `shop-demo.html` in any Browser of your choice. 
Try each test in a new incognito mode (Private). 
Use this test data **Manaf Example**, **07700900123**, and the default **London** region.


## T-101 · I pressed Pay once and I have two order numbers, both 1004. Am I being charged twice?

- **Note** Unable to reproduce this issue.

- **Steps to repeoduce**
  1. Add one item to cart (eg; Gift hamper) 
  2. Enter the default customer details.
  3. Click on **Pay now** once. 
  
- **Expected result** A thank you mesessage with one order record and one unique order number is recorded.

- **Actual result** A thank you mesessage with one order record and one unique order number is shown. 

**What I noticed** What I noticed is that when you repeatedly click on the **Pay now** button, it creates multiple orders with the same order number. I checked the **orderList** in the Local storage under DevTool to confirm this. (I have reported this as a separate bug)

## T-102 · I used the code WELCOME10 and my total was less than half of what I expected. Is the code supposed to be that big? 

Bug title: Able to reapply Promo code multiple times in one order. 

- **Severity** Major. Users are able to apply the Promo code multiple times once per order.
- **Priority** High

- **Steps to reproduce**
  1. Add three Peony bouquets (£360.00 subtotal).
  2. Enter `WELCOME10` and click **Apply** once. Record the total.
  3. Click **Apply** seven more times with the same code still entered.
  
- **Expected result** Since a Promo code can only be used once per order and delivery is free for orders above £300, a £36.00 discount, delivery of £0.00 and a total £324.00 total is expected. 

- **Actual result** When the code was applied once, a £36.00 discount and a total of £324.00 was displayed but after applying it seven more times, a £205.03 discount and a £154.97 total was displayed. This is a bug as indicated by the customer. 


## T-103 · I am in Manchester. Your page showed delivery as £20 for me, but your website says Manchester is £15. I paid £20.

Note: (This is an edge case as it only happens when the user applies the promo before changing the region to Manchester or any other for that matter)

- **Severity** Major. 
- **Priority** High

- **Steps to reproduce**
  1. Add one Gift hamper. Leave London selected, 
  2. Apply `WELCOME10` promo code, then change the region to **Manchester**.
  3. Check the delivery line(shows as £20). 
  4. Enter the default customer details
  5. Click **Pay now** once.
  
- **Expected result** Since delivery fee for Manchester is £15.00, an £15.00 delivery and a £96.00 total is expected. 

- **Actual result** Delivery stayed at £20.00. The total and paid confirmation showed £101.00.


## T-104 · I typed my number with +44 at the front and it says it is not valid. I have used that number on your website before.

Bug title: Checkout rejects valid UK mobile numbers starting with +44 country code.

- **Severity** Minor. Affected users are blocked but the workaround is to remove the country code +44.
- **Priority** High

- **Steps to reproduce**
  1. Add one Gift hamper. 
  2. Enter **Manaf Abdul** as full name
  3. Enter **mobile number** `+447700900123`.
  2. Click **Pay now**. Then replace the number with `07700900123` and retry.
  
- **Expected result** Both mobile number forms are accepted as the same UK mobile number.

- **Actual result** The mobile number with +44 form is rejected and an error “Enter a valid phone number.”


## T-105 · My bank declined the payment but your page says my order is paid. Is my order coming?

Bug title: Declined payment creates a paid order

Assumption: The Bank declined the payment as this cannot be simulated. 

- **Severity** Critical. An unpaid order could be delivered leading to lost for the shop. 
- **Priority** Highest

- **Steps to reproduce**
  1. Add one Gift hamper. Enter **Manaf Abdul** and `07700900123`. 
  2. Click **Pay now** with the assumption that the payment does not go through. 
  
- **Expected result** A decline message, with no paid record.

- **Actual result** A thank you mesessage with one order record and one unique order number is shown. 


## T-106 · On my phone the page is cut off on the right. I cannot see the whole total and I cannot find the Pay button.

Bug Title: Customers on narrow phones cannot read the full order total in the initial viewport.

- **Severity** Major. User are unable to see the Amounts to confirm before Payment.
- **Priority** Hight

- **Steps to reproduce**
  1. In Chrome mobile emulation, set the viewport to **320 × 568 CSS pixels** or use **Mobile Simulator** extension to simulate a small mobile screen and reload.
  2. Add one Gift hamper and scroll vertically to the checkout summary.
  
- **Expected result** The Subtotal, discount, delivery and Total amounts should display properly. 

- **Actual result** The Subtotal, discount, delivery and Total amounts are cut off. 


## T-107 · My total on the confirmation is one penny less than on the page before I paid. Which one is right?

Bugg Title: Paid confirmation is one penny below the quoted total

- **Severity** Major 
- **Priority** High

- **Steps to reproduce**
  1. Add three Gift hampers and two Loose leaf teas.
  2. Apply `WELCOME10`, 
  4. Enter the default customer details and note the summary total.
  3. Click **Pay now** once and compare the confirmation.
  
- **Expected RESULT** The payment confirmation and the Total in the Summary table must show the same amount of £270.90.

- **Actual result** The payment confirmation shows a Total of GBP 270.89 instead of £270.90


## T-108 · I put my name with a heart and the confirmation looked normal, but my friend tried a weird name and the page behaved strangely.

- **Severity** Minor 
- **Priority** High 

- **Steps to reproduce**
  1. Add one Gift hamper and enter the mobile number **07700900123**.
  2. Enter `Manaf <u>Abdul</u> *&&*^^&%%%%6665%` as the name 
  3. Click **Pay now**.
  
- **Expected** An error message is like "Invalid charaters in the name field" is displayed

- **Actual** The heading displayed “Thank you, Manaf <u>Abdul</u> *&&*^^&%%%%6665%! ” with **Abdul** underlined.
