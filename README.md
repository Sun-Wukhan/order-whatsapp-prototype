# OrderFlow v3 — Auto Body Supply: One Page Per Order, Submit → Confirm Flow (Simulated Live Env)

**Live:** https://sun-wukhan.github.io/order-whatsapp-prototype/

Rough prototype on GitHub Pages that **simulates a real environment before real keys are plugged in**. Dummy / hard-coded data only — no backend, no real WhatsApp, no real payments, no real keys.

## Page-to-page flow

Use the top nav / stepper — each step is its own page (hash routes, browser Back works, pages are linkable):

| Page | Route | What happens |
|---|---|---|
| 1. Shop | `#/shop` | Catalogue of 8 auto body items (paint, clear coat, HVLP gun, DA sander, sandpaper, filler, primer, masking) with simulated stock levels |
| 2. Checkout | `#/checkout` | Body-shop customer details, PO №, paint-code notes, order review |
| 3. Processing | `#/processing` | **Simulated backend**: pipeline + API log + request/response JSON |
| 4. WhatsApp | `#/whatsapp` | Mock Business WhatsApp conversation + message records |
| 5. Invoice | `#/invoice` | One invoice №, Customer Copy + Business Copy, print/PDF, download, delivery receipts |
| 6. Track | `#/track` | Customer order-status timeline |
| — Dashboard | `#/dashboard` | Business view: KPIs, all orders (seeded + new), inventory table |
| — Environment | `#/settings` | Where real keys plug in — see below |
| — Order Page | `#/order/<orderId>` | **One page per order** — see below |

## One page per order + submit/confirm navigation (v3)

- Every order gets its own page: `#/order/ORD-2026-xxxx` (e.g. `#/order/ORD-2026-1031` for a seeded order). The Dashboard's “📄 Order Page →” opens it.
- That page holds everything for that single order: KPIs, items, history, and a **Remediate / Edit** form (contact, WhatsApp №, address, fulfilment, status, PO, notes) plus quantity +/- and add-item controls.
- **Save Changes** simulates `PATCH /api/v1/orders/:id`, recalculates totals + the invoice, records a `REMEDIATED` history entry, and pushes an amended-order WhatsApp message (mock) — WhatsApp, Invoice, Tracking and Dashboard all update.
- Linear flow buttons are named for what they do: **Submit** on Shop → Checkout, **Submit Order** on Checkout → Processing, **Confirm** on Processing → WhatsApp, **Confirm** on WhatsApp → Invoice, **Confirm Invoice** → Tracking.
- Go back and forth freely: Back buttons on every page, browser Back/Forward buttons in the header, a clickable stepper, and jump buttons on the Order Page to that order's WhatsApp / Invoice / Tracking pages. State is persisted, so nothing is lost moving between pages.

## Simulated real environment

Everything production would do is simulated in-browser:

- **Simulated API** — `POST /api/v1/orders`, inventory reserve, `POST /graph/v21.0/{phone_id}/messages`, `POST /webhooks/whatsapp` (delivered/read), `POST /api/v1/invoices`, `PATCH …/status` — with fake latency, 200/201 codes, and a live API log on Page 3
- **Simulated DB** (`localStorage`) — tables: `products`, `inventory` (decrements on order), `orders`, `messages`, `invoices`, `apiLogs`; seeded with 2 historical orders
- **Simulated WhatsApp provider** — templates `order_confirmation_v1`, `invoice_v1`, `status_update_v1`; message records with `wamid.mock.*` IDs
- **Order state machine** — PLACED → WHATSAPP_SENT → CONFIRMED → PREPARING → READY → DELIVERED, plus INVOICED

## Where the keys go (Page: Environment / Keys)

`#/settings` shows the exact production variables, currently empty/masked with mode **locked to MOCK**:

- `WHATSAPP_ACCESS_TOKEN`
- `WHATSAPP_PHONE_NUMBER_ID`
- `WABA_ID` (WhatsApp Business Account ID)
- `WEBHOOK_VERIFY_TOKEN`
- `META_APP_SECRET`

“Go Live” stays disabled until keys are present and the webhook is verified — plugging them in is conceptually the only change: pages, API calls, and webhooks stay the same.

## Tech
Single `index.html`, inline CSS/JS, hash routing, no build step.
