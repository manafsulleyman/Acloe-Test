# Checkout test plan

**What I tested**
    
    1. Single payment,Simulated payment decline, Repeat submission(Duplicate orders) 
                Why => Duplicate orders can create reconciliation problems
    
    2. Quoted versus confirmed totals, promo reuse, regional delivery and pricing boundaries 
                Why => The price shown before payment must agree with the confirmation.
                
    3. Tested £200 and £300 exactly and above, then checked the £50 minimum.
                Why => The rules set for applying promos and delivery, etc must be adheres to prevent revenue lost. 
                
    4. Quantity edits and stock after fulfilment and reload 
                Why ==> A cart should only quote and accept whole quantities the shop can supply.
                
    5. UK mobile formats, name handling and checkout at small devices. 
                Why ==> Customers need to enter delivery details and read the total on small phones.

**What I did not tested**
    1. Typo errors
    2. UI misalignment
    3. User experience
    4. Multiple devices test
    

**What I will test with more time and real device.** Live payments, bank approval and cancellation, actual small phones, OS large text, Promo combinations. Add cart validation as the next automated release check.
