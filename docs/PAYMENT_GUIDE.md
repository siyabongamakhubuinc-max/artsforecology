# Arts For Ecology — Payment Integration Guide

## Overview

The donation and pledge forms capture intent and store submissions in the browser.  
To process **real money**, you need to connect a South African payment gateway.  
The recommended option is **PayFast** — the most widely used SA gateway, trusted by NGOs.

---

## Option 1 — PayFast (Recommended for SA)

### Step 1: Create a PayFast account
1. Go to https://www.payfast.co.za
2. Click **Sign Up** → choose **Business** account
3. Complete registration and verify your ID (required for NGOs)
4. Once approved, find your **Merchant ID** and **Merchant Key** in Settings

### Step 2: Set up in Admin panel
1. Go to `yoursite.com/#/admin` → **⚙ Settings** tab
2. Enter your Merchant ID and Merchant Key
3. Click **Save Payment Settings**
4. Export JSON and update your HTML file

### Step 3: Connect the Donate button

Replace the `submitDonation` function call with a PayFast redirect.  
Add this snippet to your `index.html` (replace YOUR values):

```html
<form id="payfast-form" action="https://www.payfast.co.za/eng/process" method="post">
  <input type="hidden" name="merchant_id" value="YOUR_MERCHANT_ID">
  <input type="hidden" name="merchant_key" value="YOUR_MERCHANT_KEY">
  <input type="hidden" name="return_url" value="https://artsforecology.co.za/thank-you">
  <input type="hidden" name="cancel_url" value="https://artsforecology.co.za/donate">
  <input type="hidden" name="notify_url" value="https://artsforecology.co.za/api/payfast-notify">
  <input type="hidden" name="name_first" id="pf-name">
  <input type="hidden" name="email_address" id="pf-email">
  <input type="hidden" name="amount" id="pf-amount">
  <input type="hidden" name="item_name" value="Arts For Ecology Donation">
  <input type="hidden" name="item_description" value="Supporting climate culture in South Africa">
</form>
```

Then in your JavaScript, replace the `submitDonation` toast with:

```javascript
function submitDonation(type, tier, amtStr) {
  const amount = parseFloat(document.getElementById('custom-input')?.value) || donateAmt;
  document.getElementById('pf-name').value = 'Donor';
  document.getElementById('pf-email').value = '';
  document.getElementById('pf-amount').value = amount.toFixed(2);
  document.getElementById('payfast-form').submit();
}
```

### PayFast sandbox (for testing)
Use `https://sandbox.payfast.co.za/eng/process` while testing.  
Test credentials: Merchant ID `10000100`, Merchant Key `46f0cd694581a`

---

## Option 2 — PayGate

PayGate is another SA option with good NGO support.

1. Register at https://www.paygate.co.za
2. Use their hosted payment page (Lightbox)
3. Integration docs: https://docs.paygate.co.za

---

## Option 3 — Stripe (International)

If you expect international donors, Stripe works globally.

1. Register at https://stripe.com
2. Create a **Payment Link** for each donation tier
3. Replace the tier card buttons with:

```html
<a href="https://buy.stripe.com/YOUR_LINK" class="tier-btn">Pledge Monthly</a>
```

Stripe Payment Links require no code — just paste the URL.

---

## Pledge Form — No Payment Required

The pledge form is intentionally contact-only. Pledgers express intent; you follow up  
with an invoice or EFT details. This is correct for a South African NGO context.

The pledge data appears in the Admin → Donations tab and can be exported as CSV.

---

## EFT / Bank Transfer (already live)

The bank details section is live. Donors transfer and email proof.  
**Action needed:** Update `index.html` with real account number and branch code.

Search for `62XXXXXXXXX` in the file and replace with your actual account number.

---

## Google Sheets (for form submissions)

Already set up in the code. Follow the `AFC_SETUP_GUIDE.md` instructions to connect  
your Google Apps Script URL. Every membership application and pledge will log to Sheets.

---

## Security note

Never put your **merchant secret** (different from merchant key) in frontend HTML —  
it must only live on a server. For a simple static site, use PayFast's hosted page  
(which only needs merchant_id and merchant_key, both safe to expose).
