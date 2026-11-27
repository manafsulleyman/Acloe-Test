# Step 2 · Defects report

**Note on How to start the test** 
Open `shop-demo.html` in any Browser of your choice. 
Try each test in a new incognito mode (Private). 
Use this test data **Manaf Example**, **07700900123**, and the default **London** region.


## E-01 · Cart quantity edits allow negative values. 

- **Severity** Critical
- **Priority** Highest

- **Steps to reproduce** 
    1. Add a Gift hamper and a Peony bouquet. 
    2. Set the hamper to **−1** and the bouquet to **2**. 
    3. Enter the customer details and click **Pay now**.
    
- **Expected result** The quantity field accepts whole numbers only from 1. 

- **Actual result** An error message "Please enter a valid quantity" is displayed.


## E-02 · Repeatedly clicking on Pay now creates multiple payment records with the same order number. 

- **Severity** Critical. Duplicate paid records could trigger duplicate fulfilment.
- **Priority** P0 · Fix first and block rollout.
- **Steps**
  1. Add one Gift hamper and enter the default customer details.
  2. Double-click **Pay now** quickly, before the cart clears.
  3. In DevTools under Application → Local Storage, inspect `orderList`. If needed, read it from the console with `JSON.parse(localStorage.getItem('orderList'))`.
- **Expected** One paid record and one unique confirmation number for the checkout. Both clicks act on the same cart and payment attempt.
- **Actual** Two paid records share one order number. In the browser run, a single-click control created one record (#1001), and a rapid double-click on the next cart created two records with #1002. The exact single-click report and #1004 were unverified. The demo provided no evidence of a real charge.



## E-03 · Reloading the demo makes sold-out stock available again

- **Severity** Major
- **Priority** High 

- **Steps to reproduce** 
    1. Set a Gift hamper’s quantity to **3** and complete payment. 
    2. Wait for the cart to clear and confirm the shop shows **0 in stock**. 
    3. Reload the page in the same tab, then add and buy another hamper.
    
- **Expected** Reloading refreshes the available stock. 

- **Actual result** Reload restored **3 in stock** and user was able to make second paid order. 


## E-04 · A £300.00 order receives free delivery. 

- **Severity** Major
- **Priority** High

- **Steps to reproduce**
    1. With London selected and no promo, add a Scented candle and set the quantity to **12**. 
    2. Read the delivery fee and total. 
    3. Change the quantity to **13** for comparison.
    
- **Expected result** Rules says free delivery applies to orders **over** £300.00 and my total order is £300.00 therefore London delivery is expected to be £20.00 and the total is £320.00.

- **Actual result** At 12 candles the delivery fee was £0.00 and the total was £300.00. At 13 candles the total was £325.00 with free delivery.


## E-05 · THISTLE20 discounts an order at exactly £200.00

- **Severity** Major
- **Priority** High 

- **Steps to reproduce** 
    1. With London selected, add a Scented candle and set the quantity to **8**. 
    2. Apply `THISTLE20`
    3. Observe there is a discount amount applied in the summary page. 
    
- **Expected result** No discount amount is applied since the total amount is exactly £200.00 and per the rule THISTLE20 applies for total amount over £200.00

- **Actual** An amount of £200.00 got the £20.00 discount and totalled £200.00 instead of £0.00 discount.


## E-06 · Checkout applies WELCOME10 and THISTLE20 to the same order

- **Severity** Major
- **Priority** High
- **Steps to reproduce**

  1. Add two Peony bouquets to the cart for a subtotal of **£240.00**. Leave **London** selected for **£20.00** delivery.
  2. Enter `WELCOME10` and click **Apply**. Confirm that the discount is **£24.00** and the total is **£236.00**.
  3. Replace `WELCOME10` in the promo field with `THISTLE20` and click **Apply**.
  4. Check the discount and total in the checkout summary.

- **Expected result** Checkout applies only one promo code per order. If it keeps `WELCOME10`, the total stays at **£236.00**. If it replaces `WELCOME10` with `THISTLE20`, the total becomes **£240.00**. The page clearly indicates which code is active.
- **Actual result** Checkout applies both discounts to the same order. The discount increases to **£44.00** and the total drops to **£216.00**.


## E-07 · A whitespace-only name can complete checkout

- **Severity** Low. 
- **Priority** Meduim · 

- **Steps to reproduce** 
    1. Add one Gift hamper. 
    2. Enter three spaces in **Full name**, 
    3. Enter the default mobile number. 
    4. Click **Pay now**.
    
- **Expected result** Prompt the user to fill in their name and trim leading and trailing whitespace. 

- **Actual result** The order was marked paid and the confirmation displayed “Thank you, !”.


## E-08 · Users are able to apply the Promo code more than once per order.

- **Severity** Major
- **Priority** High

- **Steps to reproduce**
  1. Add three Peony bouquets (£360.00 subtotal).
  2. Enter `WELCOME10` and click **Apply** once. Record the total.
  3. Click **Apply** seven more times with the same code still entered.
  
- **Expected result** A single £36.00 discount is applied per order as per the rule, free delivery and a £324.00 total.

- **Actual result** Users is able to apply a discount multiple times in the same order. 


## E-09 · The Full name field accepts excessively long names

- **Severity** Minor
- **Priority** Medium

- **Steps to reproduce**
  1. Add one Gift hamper to the cart.
  2. Paste a 300-character name into the **Full name** field.
  3. Enter `07700900123` in the **Mobile number** field and click **Pay now**.

- **Expected result** The field enforces an agreed maximum length and shows a clear validation message when the name exceeds it.
- **Actual result** The field accepts the 300-character name and checkout completes without a length validation message.


## E-10 · The Full name field accepts names made up entirely of symbols

- **Severity** Minor
- **Priority** Medium

- **Steps to reproduce**
  1. Add one Gift hamper to the cart.
  2. Enter `!@#$%` in the **Full name** field.
  3. Enter `07700900123` in the **Mobile number** field and click **Pay now**.

- **Expected result** Checkout prompts the customer to enter a valid name. The field still accepts common name punctuation, such as apostrophes and hyphens.
- **Actual result** Checkout accepts the symbols-only value, completes the order, and displays the value in the confirmation.


## E-11 · The mobile number placeholder is cut off

- **Severity** Minor
- **Priority** Low

- **Steps to reproduce**
  1. Open `shop-demo.html` and scroll to **Checkout**.
  2. Leave the **Mobile number** field empty and read its placeholder.

- **Expected result** The full hint, “Mobile number (for delivery updates),” is readable.
- **Actual result** Part of the placeholder is clipped, so the full delivery-update hint cannot be read.


## E-12 · Customers can order more items than are in stock

- **Severity** Critical
- **Priority** High

- **Steps to reproduce**
  1. Open the demo in a fresh session and confirm that the Gift hamper shows **3 in stock**.
  2. Add one Gift hamper to the cart, change its quantity to **4**, and press **Tab**.
  3. Enter **Manaf Abdul** and `07700900123`, then click **Pay now**.

- **Expected result** Checkout prevents the order and explains that only three hampers are available.
- **Actual result** Checkout marks the order as paid for four hampers. After the cart clears, the shop shows **−1 in stock**.


## E-13 · Checkout and order confirmation use different currency formats

- **Severity** Minor
- **Priority** Low

- **Steps to reproduce**
  1. Add one Gift hamper to the cart and leave **London** selected.
  2. Note the total in the checkout summary.
  3. Enter **Manaf Abdul** and `07700900123`, then click **Pay now**.
  4. Compare the confirmation total with the checkout total.

- **Expected result** Both screens use the same currency format, for example **£110.00**.
- **Actual result** The checkout displays **£110.00** and the confirmation displays **GBP 110.00**. The amount matches, but the currency format changes.


## E-14 · Clicking Remove immediately after editing a quantity does not remove the item

- **Severity** Minor
- **Priority** Medium

- **Steps to reproduce**

  1. Add one Gift hamper to the cart.
  2. Change its quantity from **1** to **2** and leave the cursor in the quantity field.
  3. Click **Remove** once, then click it again if the item is still present.

- **Expected result** The first click removes the item from the cart.
- **Actual result** The first click commits the quantity change and updates the line price to **£180.00**. The item stays in the cart until the second click.

## E-15 · An invalid promo attempt hides which discount is still applied

- **Severity** Minor
- **Priority** Medium
- **Steps to reproduce**

  1. With London selected, add one Gift hamper.
  2. Apply `WELCOME10` and confirm the summary shows a **£9.00** discount.
  3. Replace `WELCOME10` in the promo field with `WRONG` and click **Apply**.

- **Expected result** Checkout identifies the new code as invalid and clearly shows that `WELCOME10` is still active if its discount is retained.
- **Actual result** The promo message changes to “Invalid code,” but the **£9.00** discount remains and the total stays at **£101.00**. The page no longer identifies the code responsible for the discount.
