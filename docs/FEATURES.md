# Features walkthrough

A tab-by-tab tour of what the dashboard does. Try any of it live on the [seeded demo](https://patrickdugo.github.io/svs-command-center/?demo).

## 🏠 Today
The morning glance: revenue-to-date against the monthly milestone, upcoming shoots and subscription visits, deliveries due, and the few things that need attention now. Pulls live from every other module.

## 👥 Clients
The heart of the tool.

- **Pipeline** — every lead as a card, moving through *lead → booked → delivered → closed* with one tap. Booking a deposit auto-advances a lead to *booked*.
- **Client card** — package/fee, auto-half deposit, balance, event date, venue, start/end times, expected image count, delivery SLA (days), brief and notes.
- **Documents** — generate an invoice, contract, receipt or delivery note for that client in one click, formatted for print/PDF and branded from your Setup details.
- **Calendar** — one unified view of one-off shoots and recurring subscription visits.

## 💰 Money
- **Record a payment** and it is automatically split **50 / 20 / 15 / 15** across Personal, Studio Growth, Tax Provision and Emergency Buffer, then written to the ledger as four tagged lines.
- Track income, expenses and a running balance per bucket.
- Progress bars against a ladder of monthly revenue milestones.
- **Backup** exports the whole business to one JSON file; **Import** restores it on another device.

## 🔁 Subscriptions
For recurring retainer clients (a "Business Online Pack"):

- Add a client with their location and monthly visit count.
- The **auto-scheduler** batches clients by area onto specific weekdays, packs visits into two 3-hour blocks per day, respects a weekly-hours target and an active-client cap, and avoids collisions.
- Any visit can be **locked and hand-edited**; the optimiser schedules around locked slots.

## 🎬 Studio
Delivery tracker with SLA countdowns, a pre/post-shoot backup checklist, and a risk register — the operational hygiene that keeps a solo studio from dropping balls.

## 🌱 Grow &amp; Learn
- A month-by-month revenue plan tied to services, pricing and skill-building.
- A task board of the highest-leverage actions.
- An embedded, structured **editing &amp; photography course** (module → lesson) for levelling up craft.

## ⚙️ Setup
Enter the business once — name, contact, MoMo details, money-split percentages, packages and prices, milestones — and it flows into the header, the pricing sheet, every generated document, and the payment splitter. One source of truth.

## Cross-cutting
- **Security** — passcode + one-time recovery code, AES-GCM encrypted storage, auto-lock (see [ARCHITECTURE](ARCHITECTURE.md)).
- **Responsive** — phone-first; "Add to Home Screen" gives an app icon.
- **Offline** — everything above works with no connection.
