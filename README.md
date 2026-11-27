# Kerbwise checkout QA

Run these commands from the folder containing `shop-demo.html`, `checkout.spec.js`, and `checkout-data.js`. Use Node.js 22 with npm.

```sh
npm install --no-save --package-lock=false @playwright/test@1.55.1
npx playwright install chromium
npx playwright test checkout.spec.js --workers=1
```
