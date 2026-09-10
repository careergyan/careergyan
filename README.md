# CareerGyan Event Frontend

Static frontend for the CareerGyan Abroad Education Fair – Intake 2026 registration page.

## Included
- `public/index.html` — registration page with Google reCAPTCHA and Razorpay checkout integration.
- `public/privacy-policy.html` — privacy policy page.
- `public/terms-and-conditions.html` — terms and conditions page.
- `vercel.json` — Vercel configuration.

## Supabase frontend configuration
The frontend is configured with the supplied Supabase project URL and publishable key and calls these Edge Functions:
- `create-order`
- `verify-payment`

Supabase URL:
`https://veovdcqtyrbakyednkvr.supabase.co`

The Supabase publishable key is safe to expose in browser code. Never expose Supabase secret/service-role keys, Razorpay Key Secret, Razorpay Webhook Secret, Resend API key, or reCAPTCHA Secret in this project.

## Payment flow
1. User completes the form and reCAPTCHA.
2. Frontend calls `create-order` with registration details.
3. The Edge Function creates the Razorpay order and returns the order ID, amount, currency and Razorpay Key ID.
4. Razorpay Checkout opens in the browser.
5. After payment, frontend calls `verify-payment` with the Razorpay response and registration ID.
6. Server-side verification/webhook should update the `registrations` table and trigger the confirmation email flow.

The frontend accepts common response names from the Edge Functions (`registrationId`/`registration_id`, `orderId`/`order_id`, `keyId`/`key_id`) to make the integration tolerant of naming differences.

## Vercel
Import this repository into Vercel. The site is static, so no Render server is required for the frontend.

