# PR review for Kwesi

**Request changes.** 

1. Isolate the test data from the spec file. You need to keep all the test data in a separate file and reference them in the test file. 
**reference:**
    await page.fill('#name', 'Alice Okafor')
    await page.fill('#phone', '07700900123')

2. Set the retries to 2 (which in actual runs is 3). 5 retries is way too much. If the test is failing on even the 3 tries, you need to investigate. 
**reference:**
    retries: 5 

3. Avoid await page.waitForTimeout(3000) in tests. This will always waits three seconds, even if the page is ready immediately and if the page takes longer than three seconds, the next step can still fail.
**reference:**
    await page.waitForTimeout(3000)

4.  Locate each product card by name and click its Add to cart button. That way, reordering the catalogue won’t change what this test buys.
**reference:**
    await page.click('div.products > div:nth-child(4) > button')
    await page.click('div.products > div:nth-child(3) > button')
    
5. Do not skip a test when a bug is found by automation. Rather, link the known defect and allow the check with the fix. 
**reference:**
    test.skip('phone check', async ({ p
