SHARMA FAST FOOD — ADMIN + ONLINE PAYMENT
=============================================

ADMIN
-----
Open admin.html
Demo password: admin123

IMPORTANT: This demo dashboard uses browser localStorage so it works immediately,
but it is NOT a secure production admin system. For production, replace the
demo login with server-side authentication and a database.

ONLINE PAYMENT
--------------
The storefront already has payment choices:
1) WhatsApp / Cash — works immediately.
2) Stripe — checkout button is integration-ready but requires a server.
3) PayPal — integration-ready but requires a server.

DO NOT put Stripe secret keys or PayPal secrets in index.html/script.js.

Recommended production architecture:
Frontend -> POST /api/create-checkout-session -> Server -> Stripe/PayPal
Payment provider -> POST /api/webhook -> Server -> Database -> Admin dashboard

STRIPE
------
Create a Stripe account and configure:
STRIPE_SECRET_KEY=...
STRIPE_WEBHOOK_SECRET=...
Then create a server endpoint /api/create-checkout-session.
The frontend should send cart item IDs and quantities to that endpoint.
The server validates prices from its database and creates the Stripe Checkout Session.

PAYPAL
------
Configure:
PAYPAL_CLIENT_ID=...
PAYPAL_CLIENT_SECRET=...
Use PayPal Orders API from the server. Never trust price totals supplied by
the browser; calculate totals on the server.

DATABASE
--------
For real orders/reservations, use PostgreSQL, MySQL, Supabase, Firebase, or
another backend. Store:
orders(id, customer_name, phone, address, total, payment_status, provider,
provider_reference, created_at)
order_items(order_id, menu_item_id, quantity, unit_price)
reservations(id, name, phone, date, time, guests, note, status)

DEPLOYMENT
----------
Static-only hosting (GitHub Pages/Netlify/Vercel static) is enough for the
WhatsApp version. Real card payments require a backend/serverless function
and HTTPS.
