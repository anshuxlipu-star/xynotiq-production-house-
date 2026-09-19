# XYNOTIQ Production House

A ready-to-customize 90s-inspired production-house website for editing + directing.

## Included
- Aesthetic 90s / VHS / film-strip design with animated illustrations, marquee text and CRT scanlines.
- Pre-booking form for Editing, Director, and Editing + Direction.
- Contract display: **$100/edit = $45 advance + $55 final**.
- Razorpay Standard Checkout integration hooks for card/UPI where your Razorpay account supports the configured currency/methods.
- Private client room per paid booking: chat + file upload + final-video delivery link.
- Admin deck: booking list, status updates, client chat, client file uploads, final-link unlock.
- Excel automation hook via Power Automate: after verified payment, the server POSTs a structured row to your flow.
- Excel template: `xynotiq_production_bookings_template.xlsx` (see `excel_setup.md`).

## Important payment note
The site displays your contract in USD because that is what you specified. Razorpay/UPI settlement currency and supported payment methods depend on your Razorpay account and configuration. Do **not** assume that a $45 USD amount can be charged through Indian UPI. Set `RAZORPAY_CURRENCY` and the smallest-unit gateway amounts explicitly after you confirm what your Razorpay account supports.

Manual UPI shown on the public site: `7077985617@nyes`.
A manual UPI transfer is not automatically verified by this starter; automated Excel logging is tied to a verified gateway payment/webhook.

## Local setup
1. Install Node.js 20+.
2. Copy `.env.example` to `.env` and fill in your values.
3. Run `npm install`.
4. Run `npm start`.
5. Open `http://localhost:3000`.
6. Admin: `http://localhost:3000/admin.html`.

## Razorpay setup
Create an Order on the server, open Standard Checkout in the browser, verify the checkout signature on the server, and configure webhooks such as `payment.captured` / `order.paid`. The server code follows this pattern and does not expose the API secret to the browser.

Set:
- `RAZORPAY_KEY_ID`
- `RAZORPAY_KEY_SECRET`
- `RAZORPAY_WEBHOOK_SECRET`
- `RAZORPAY_CURRENCY`
- `RAZORPAY_ADVANCE_AMOUNT_MINOR`
- `RAZORPAY_FINAL_AMOUNT_MINOR`

## Excel automation
Use the supplied workbook with Power Automate. The workbook contains a `Bookings` table. Your flow should receive a JSON payload at an HTTP trigger and use **Excel Online (Business) → Add a row into a table** to append the `row` object from the request. See `excel_setup.md`.

For production, put the Node app behind HTTPS, move uploads to durable cloud storage, use a real user/auth system, and use a long random `ADMIN_TOKEN`.
