# 10to2 payment backend

This folder is a **copy for safekeeping** — these files belong in your
`10to22/Payment` repo (the one Vercel builds as
`payment-swart-psi.vercel.app`), not in the website repo. GitHub Pages
ignores them; they only run once deployed to Vercel.

```
api/
  _catalog.js               server-side prices + Printful variant IDs (source of truth)
  create-payment-intent.js  re-prices the cart, makes the Stripe PaymentIntent (has CORS)
  stripe-webhook.js         on payment success, creates the Printful order
  printful-variants.js      OPTIONAL: lists your Printful IDs in the browser (delete after use)
.env.example
package.json
```

## Deploy (fixes the CORS error, no local tools needed)

The CORS error you saw —
`No 'Access-Control-Allow-Origin' header` — means the version currently live
on Vercel doesn't send CORS headers. `create-payment-intent.js` here does
(it sets the headers and answers the `OPTIONS` preflight), so deploying it
makes the error go away.

1. Go to **github.com/10to22/Payment**.
2. Add/replace these files (`Add file -> Upload files`, or edit in place).
   **Replace** the existing `api/create-payment-intent.js` with this one.
3. Commit -> Vercel auto-deploys in ~1 min.
4. **Vercel -> Settings -> Environment Variables** — set:
   - `STRIPE_SECRET_KEY`      (sk_test_... while testing, sk_live_... for real)
   - `STRIPE_WEBHOOK_SECRET`  (whsec_... from the Stripe webhook, step below)
   - `PRINTFUL_API_TOKEN`     (Printful -> Settings -> Stores -> API)
   - `PRINTFUL_STORE_ID`      (only if your token spans multiple stores)
   - `PRINTFUL_CONFIRM_ORDERS`(leave unset to hold orders as drafts you approve)
   Redeploy after adding them.
5. Reload 10to2.net and try checkout — the CORS block should be gone.

## Stripe webhook (so paid orders actually reach Printful)

Stripe Dashboard -> Developers -> Webhooks -> Add endpoint:
- URL: `https://payment-swart-psi.vercel.app/api/stripe-webhook`
- Event: `payment_intent.succeeded`
- Copy the signing secret (`whsec_...`) into `STRIPE_WEBHOOK_SECRET`.

## Printful billing

Printful -> Billing -> add a card or top up the Wallet, or paid orders
can't be charged to you and won't ship.

## Verify your Printful variant IDs

Deploy `printful-variants.js`, then open
`https://payment-swart-psi.vercel.app/api/printful-variants` in a browser.
For each size, use the numeric `id` in `_catalog.js`. (If the hashes in
`_catalog.js` already appear as `id`, they were correct — change nothing.)
Delete this file once you've grabbed the IDs.

## Test before going live

- Use Stripe **test** keys + a test-mode webhook secret.
- On 10to2.net, buy the tee with card `4242 4242 4242 4242`.
- Check: Stripe shows a succeeded test payment -> Vercel logs show
  "Printful order created" -> a draft order appears in Printful with the
  right size and address.
- Then swap all keys to live and (optionally) set `PRINTFUL_CONFIRM_ORDERS=true`.

## Why prices are recomputed server-side

`create-payment-intent.js` ignores the prices the browser sends and rebuilds
every amount from `_catalog.js`. Otherwise anyone could edit a price in dev
tools and check out for a penny. Keep `_catalog.js` prices and shipping in
sync with `MERCH` / `BACKEND_CONFIG.shipping` in the site's index.html.
