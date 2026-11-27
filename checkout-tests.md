# Kerbwise Playwright checkout checks

Keep both JavaScript files beside `shop-demo.html`. See `README.md` for run instructions.

## checkout-data.js

```js
module.exports = {
  customerName: 'Manaf Abdul',
  validPhone: '07700900123',
  declinedPhone: '07700900120', // The demo declines numbers ending in 0.
  product: 'Gift hamper',
  region: 'Manchester',
  deliveryFee: '£15.00',
  checkoutTotal: '£105.00',
  confirmationTotal: 'Total: GBP 105.00',
  stockAfterOneOrder: '2 in stock',
};
```

## checkout.spec.js

```js
const { test, expect } = require('@playwright/test');
const { resolve } = require('node:path');
const { pathToFileURL } = require('node:url');
const data = require('./checkout-data');

const demoFile = process.env.CHECKOUT_FILE || resolve(__dirname, 'shop-demo.html');
const productCard = page => page.locator('.card').filter({ hasText: data.product });
const mobileNumber = page => page.getByPlaceholder('Mobile number (for delivery updates)');
const payNow = page => page.getByRole('button', { name: 'Pay now' });

test.beforeEach(async ({ page }) => {
  await page.goto(pathToFileURL(resolve(demoFile)).href);
  await productCard(page).getByRole('button', { name: 'Add to cart' }).click();
  await page.getByPlaceholder('Full name').fill(data.customerName);
});

test('completes one order at the quoted total', async ({ page }) => {
  await mobileNumber(page).fill(data.validPhone);
  await page.locator('#region').selectOption({ label: data.region });

  const summary = page.locator('#summary');
  await expect(summary.getByRole('row', { name: /^Delivery/ })).toContainText(data.deliveryFee);
  await expect(summary.getByRole('row', { name: /^Total/ })).toContainText(data.checkoutTotal);

  await payNow(page).click();

  const confirmation = page.locator('#confirm');
  await expect(confirmation).toContainText(data.customerName);
  await expect(confirmation.getByText(/^Order #\d+ · status: paid$/)).toBeVisible();
  await expect(confirmation.getByRole('listitem')).toHaveText(`1 × ${data.product}`);
  await expect(confirmation.getByText(/^Total: GBP /)).toHaveText(data.confirmationTotal);
});

test('a declined payment does not show a paid confirmation', async ({ page }) => {
  await mobileNumber(page).fill(data.declinedPhone);
  await payNow(page).click();

  await expect(page.getByText(/payment declined/i)).toBeVisible();
  await expect(page.locator('#confirm')).not.toContainText('status: paid');
});

test('double-clicking Pay now deducts stock only once', async ({ page }) => {
  await mobileNumber(page).fill(data.validPhone);
  await payNow(page).dblclick({ delay: 50 });

  await expect(page.locator('#confirm')).toContainText('status: paid');
  await expect(page.locator('#cart')).toContainText('Your cart is empty.');
  await expect(productCard(page)).toContainText(data.stockAfterOneOrder);
});
```
