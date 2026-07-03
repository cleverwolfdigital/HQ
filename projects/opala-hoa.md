# Opala Hoa — Marketplace

**Outcome:** A working, tested marketplace/auction that completes real transactions with unit economics that don't lose money.
**Status:** active (FOCUS) — labeled "operating" but functionally live-pre-revenue (no transactions yet)
**Category:** Project
**Owner:** Quincy (100%)
**Deadline:** 90-day goal set 2026-07-01
**Client:** internal
**Related:** [PORTFOLIO.md](../PORTFOLIO.md), https://github.com/[org]/opalahoa, https://www.opalahoa.com

---

## Next action
> Fix the cash register. Live + code audit (2026-07-02) found the site CANNOT complete a card sale -- this is why there are zero transactions. See Diagnostic below. Then answer the freight-margin question.

---

## Tech state (built)
- Live at opalahoa.com on Railway · Manus → Railway migration complete
- Auth: email/password + **Google OAuth** · Storage: Cloudflare R2 · Payments: Square · DB: TiDB (referral tables migrated)
- **Still has bugs to fix** before launch.

---

## Diagnostic (2026-07-02) -- why there are ZERO transactions
Live browser walkthrough of opalahoa.com + full code audit of `C:\Users\tidas\opalahoa`. Infra is healthy (Railway up, TiDB connected, API 200s, email login works, listing pages + Buy Now modal well-built). The problem is the checkout. **The site looks finished but the cash register is broken in 3 independent ways.**

### BLOCKERS -- cannot take real money
1. **Card checkout won't load.** Live: Square Web SDK 503s -> "Failed to load Square SDK," pay button stuck. Root cause in code: server falls back to Square **Sandbox** with empty appId/locationId unless `SQUARE_ENVIRONMENT="production"` + IDs set in Railway. `server/squareRouter.ts:14-17`, `transactionRouter.ts:18-20`. **Fix = Railway env vars (Quincy).**
2. **Coupon overcharge.** Buyer sees discounted total; server charges FULL price -- coupon shown, never applied/recorded. `BuyNowModal.tsx:94-97,184-188` + `transactionRouter.ts:39,55-62`.
3. **Premium is free.** `user.upgradeTier` has no payment gate -- any logged-in user upgrades to seller/premium for free, nullifying paid membership. `server/routers.ts:357-363`.

### HIGH -- money-integrity
4. **Double seller payout** if buyer confirms receipt before seller confirms sale. `transactionRouter.ts:126-142,148-167`.
5. **Escrow never auto-releases** (UI promises 7-day auto-release; nothing reads `escrowReleaseAt`) -> seller funds can be stuck forever. `transactionRouter.ts:74-88`.
6. **Off-platform fraud farm** -- Venmo/Zelle/PayPal/cash marks txn `completed` at full price AND awards referral commission with zero payment verification. `transactionRouter.ts:44-45,96`.

### MEDIUM / LOW
7. Fake homepage stats ("2,400+ listings / 12,000+ buyers" vs. 1 real listing) -- trust killer.
8. Analytics dead (`VITE_ANALYTICS_ENDPOINT` unset -> `%VITE_ANALYTICS_ENDPOINT%/umami` 503).
9. `crypto.randomUUID()` used without import -> breaks uploads on older Node. `storage.ts:38`.
10. Google OAuth links accounts by unverified email. `_core/googleOAuth.ts:145-161`.
11. Header overlap on scroll; Buy Now dialog missing a11y title; signup route not rate-limited; DB write on every API request.

### Fix order (revenue-first) + owner
1. Square env config in Railway -- **Quincy** (no repo access needed)
2. Gate `upgradeTier` behind payment -- **code**
3. Apply coupon server-side -- **code**
4. Escrow auto-release job + double-payout guard -- **code**
5. Off-platform: verify-or-don't-complete -- **code**
6. Real stats, analytics var, header/a11y -- **code (cosmetic)**

## Intake (2026-07-01)
1. **One line:** Marketplace + online auction reselling cleaned-up used equipment, plus new hard-to-find equipment/items.
2. **Stage:** Says "operating, want to scale" — but no transactions yet, so really **live pre-revenue**.
3. **Money:** Unknown — "may be losing money." Costs = Railway/TiDB/Square + physical acquisition/cleaning/freight.
4. **Ownership:** Quincy — 100%.
5. **What exists:** Entity ✓ · Live site ✓ · Users ✗ · Listings ✗ · Transactions ✗ · Bugs remain.
6. **Market:** Buyers = people wanting affordable items. Sellers = people offloading equipment/things.
7. **90-day goal:** _(drifted to the whole portfolio)_ — all businesses running consistently, **have an office / a place of operations.**
8. **#1 bottleneck:** "Myself" + the cost of **moving items** from where they come from — logistics/freight is very expensive.
9. **Your time:** Not intentional; only when it comes up.
10. **Daily 1–2:** Fix bugs, then market slowly — maybe invite-only.

---

## The honest read (flagged 2026-07-01)
- **This is the only inventory/logistics-heavy business in your focus 5.** Content businesses cost time; this one costs *cash* — acquire, clean, store, and **freight** physical equipment. You named freight as the bottleneck, and for used-equipment resale that's the whole ballgame: if it costs more to move an item than its resale margin, the model doesn't work at low price points.
- **It's the one business that burns money while idle** (Railway + TiDB + Square + any held inventory) with $0 coming in. So it's either the highest-urgency to validate or the first candidate to pause spend on.
- **Validate before you scale:** one real transaction that clears a profit after freight tells you more than fixing every bug. Consider leading with *"new hard-to-find equipment"* (higher margin, ship-direct, no cleaning) over bulky used gear if freight is killing you.

## ⚠ Bigger signal (goal drift)
Your Opala Hoa "90-day goal" wasn't about Opala Hoa — it was *"all my businesses running consistently, an office, a place of operations."* **That's your real north star for the whole portfolio, not this one business.** Captured for `the-thing-to-figure-out` / a portfolio-level goal. And the bottleneck "myself" is now the through-line across all 5 businesses.

---

## Log
| Date | Update |
|---|---|
| 2026-06-18 | Migration complete. Nav signup button PR open. |
| 2026-07-01 | Intake captured. Live site, zero transactions, bugs remain, likely burning cash. Freight economics = the existential question. Goal answer revealed a portfolio-level north star (office + consistency). |
| 2026-07-02 | Full diagnostic (live + code audit). Cash register broken 3 ways: Square SDK won't load, coupon overcharge, free premium bypass. Plus double-payout, escrow never releases, off-platform fraud farm. This explains zero transactions. Fix order recorded above. |

---

## Open questions
- [ ] What's the freight cost on a typical item vs. its resale margin? (make/break)
- [ ] Which line is the real business — cleaned used gear, or new hard-to-find? (different economics)
- [ ] Monthly infra + holding cost right now (what idle is costing you)?
- [ ] TiDB IP allowlist — add OpenClaw VPS IP to unblock agent access _(carryover)_
