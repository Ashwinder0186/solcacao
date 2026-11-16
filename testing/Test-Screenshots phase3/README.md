# Test Screenshots – SolCacao Phase 3 (Team 14)

These screenshots were taken from a local build of the code in `CodeBase/`, with the React client calling the Phase 3 API contract. The live EC2 site still runs the code from before these fixes, so it will look like the "before" shots until it is redeployed.

## 1-Fixed-App-Walkthrough

| File | What it shows | Result |
|---|---|---|
| 01_home | Home page loads with products from the API | Pass |
| 02_home_spanish | EN → ES toggle translates navigation and hero text | Pass |
| 03_shop_catalog | Products listed from `GET /api/products`, 6 per page | Pass |
| 03b_shop_page2_pagination | Page 2 of 2 with Previous / Next buttons | Pass |
| 04_shop_filter_by_origin | Filter by origin (Ecuador + Madagascar) shows 2 products | Pass |
| 05_product_detail | Product page from `GET /api/products/:id` | Pass |
| 06_cart_drawer_qty5_after_fast_clicks | Add to cart, then 4 fast "+" clicks → quantity 5, $62.50 | Pass |
| 07_cart_page | Cart page with 2 items and correct subtotal | Pass |
| 08_checkout_validation_focus_first_error | Empty/invalid fields show messages; focus jumps to the first error | Pass |
| 09_checkout_valid_st_louis_zip | "St. Louis" and ZIP 63101 accepted; all labels visible | Pass |
| 10_order_confirmation | Order placed, order number shown, cart emptied | Pass |
| 11_sampler_4_selected | Build-a-Sampler with 4 pieces selected and total | Pass |
| 12_b2b_quote_wizard | B2B custom-order wizard, step 1 | Pass |
| 13_contact_success | Contact form submitted, success message shown | Pass |
| 14_login_dialog_focus | Login dialog opens with focus inside it | Pass |
| 15_skip_to_content_link | First Tab press shows "Skip to main content" | Pass |
| 16_mobile_home_375 / 17_mobile_shop_375 | Phone layout at 375 px | Pass |

## 2-Before-vs-After-Fixes

Each pair uses the same steps on the original code (`1-before`) and the fixed code (`2-after`).

| Pair | Before | After |
|---|---|---|
| A – 4 fast "+" clicks from quantity 1 | Cart showed fewer than 5 (clicks lost) | Shows 5 |
| B – Checkout with City "St. Louis", ZIP "!!!", bad card number | No visible labels; "St. Louis" rejected; "!!!" accepted | Labels visible; "St. Louis" accepted; bad ZIP and card number rejected |
| C – Cart at 375 px | Page 437 px wide (sideways scroll) | Fits (375 px) |
| D – Cart at 320 px | Page 437 px wide | Fits (320 px) |
| E – Checkout at 320 px | Page 363 px wide | Fits (320 px) |

## API tests

The server has 60 automated API tests (Vitest + Supertest) in `CodeBase/server/tests/`. 12 of them cover the issues below: new cases in `cart.test.js`, `orders.test.js`, `products.test.js` and `auth.test.js`, plus `concurrency.test.js` (parallel adds, double checkout submit, total rounding). Run them with `cd CodeBase/server && npm install && npm test` (needs MongoDB at `MONGO_URI_TEST`).

## Issue log

| # | Issue found on the live site | Fix | Status |
|---|---|---|---|
| 1 | All guests on the HTTP site shared one cart (`crypto.randomUUID` is not available without HTTPS) | Fallback ID generator in `guestSession.js` | Fixed |
| 2 | Fast "+" clicks overwrote each other | Quantity buttons disabled while an update is in flight | Fixed |
| 3 | Parallel adds of the same product could be lost on the server | Atomic `$inc` / `$push` in `addItem` | Fixed |
| 4 | Stock never went down after an order | Stock checked and decremented at checkout, restored if the order fails | Fixed |
| 5 | Double-clicking "Place order" could create two orders | Cart claimed atomically before the order is created | Fixed |
| 6 | Bad product ID or cart quantity caused a 500 error | Zod validation on cart routes; invalid IDs return 400 | Fixed |
| 7 | Search with `(` or `*` caused an error | Search text escaped | Fixed |
| 8 | No pagination | `?page=&limit=` on `/api/products`; 6 products per page in the UI | Fixed |
| 9 | Guests saw a 401 console error from `/auth/me` | Returns 200 with `user: null` | Fixed |
| 10 | "St. Louis" rejected, any ZIP accepted | City allows periods; ZIP must be 12345 or 12345-6789 on client and server | Fixed |
| 11 | Checkout labels hidden, no focus on first error, no card checksum, no skip link, focus not kept in dialogs, cart/checkout overflow on phones | Accessibility and layout fixes | Fixed |
