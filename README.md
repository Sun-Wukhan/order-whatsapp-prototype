# OrderFlow — Auto Body Supply: WhatsApp Order + Invoice Prototype

Rough prototype hosted on GitHub Pages. **Dummy data only — no backend, no real WhatsApp, no real payments.**

**Live:** https://sun-wukhan.github.io/order-whatsapp-prototype/

Built for an **auto body supply shop** — paints, spray guns, sanders, fillers, primers and consumables. Customers are typically body shops / garages.

## Flow

1. **Take order from customer** — catalogue with 8 dummy auto body items (2K basecoat, clear coat, HVLP spray gun 1.3mm, DA orbital sander, sandpaper, body filler, primer surfacer, masking kit), cart, checkout form pre-filled with a demo body-shop customer (Mike's Collision & Repair).
2. **Process through Business WhatsApp (mocked / locked)** — no WhatsApp connector is connected, so the conversation is simulated in a phone UI:
   - Customer sends order via WhatsApp bubble
   - Business confirms with order № + total
   - Invoice message sent in-chat
   - Extra buttons simulate a customer reply and a "Preparing" update
3. **Invoice for both parties** — one invoice № (`INV-2026-xxxx`) generates two identical copies:
   - 👤 Customer Copy (for the body shop)
   - 🏪 Business Copy (for the supply shop)
   - Print / Save PDF, or Download `.txt`
   - Totals include subtotal, delivery, and HST 13% (Ontario demo)

## Tech
Single `index.html` — inline CSS/JS, no build step, `localStorage` for recent orders.

## To make it real later
- Connect WhatsApp Business Platform API (replace mock in `renderWhatsApp()`)
- Add real SKU / paint colour-code catalogue and inventory
- Add a backend / form endpoint for real orders
- Generate real PDFs server-side and send the PDF/link via WhatsApp to customer + business
