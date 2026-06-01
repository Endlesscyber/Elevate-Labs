# Phishing Email Analysis Report

## Objective

To analyze a suspicious email sample and identify phishing characteristics from a real-world phishing email (`sample-1510.eml`).

---

## Sample Email Summary

The analyzed email claimed to be from **"Express Pharmacy"** and offered discounted medications (Viagra, Cialis, etc.), urging the recipient to click a link or respond via email. It was sent from a **Hotmail address** with a display name containing product prices, targeting unsuspecting recipients.

### Email Rendered View

![Email Screenshot](email_screenshot.png)

---

## Email Header Details

| Field         | Value |
|---------------|-------|
| **From**      | <ex.pharmacy.online1@hotmail.com>` |
| **To**        | Undisclosed recipients |
| **Subject**   | `Your 5% markdown voucher.` |
| **Date**      | Fri, 06 Oct 2023 10:00:08 +0100 |
| **Return-Path** | `ex.pharmacy.online1@hotmail.com` |
| **X-Mailer**  | Microsoft Outlook 16.0 |

---

## Phishing Indicators Identified

### 1. Suspicious Sender Address

- The email originates from `ex.pharmacy.online1@hotmail.com`, a free Hotmail account, not an official pharmacy domain.
- Legitimate pharmaceutical companies use their own branded domains.

### 2. Header Discrepancies & Authentication Failures

The email header analysis revealed critical authentication failures:

![Header Analysis](header_analysis.png)

- **SPF: FAIL** — Sender IP `49.43.4.43` is not authorized to send mail for `hotmail.com`.
- **DKIM: None** — The message was **not signed**, meaning authenticity cannot be verified.
- **DMARC: FAIL** — Combined authentication check failed (`compauth=fail, reason=001`).
- **Return-Path** mismatches the displayed sender identity.

### 3. Urgent / Manipulative Language

- The subject line **"Your 5% markdown voucher"** implies a personal, exclusive offer to lure clicks.
- The email presents **"Yes, I want a discount"** and **"No, I don't need Viagra/Cialis"** as binary choices — a psychological pressure tactic.
- Legitimate organizations do not phrase opt-ins as fear-of-missing-out choices.

### 4. Suspicious Links

![Links Analysis](links_analysis.png)

The email contains three deceptive links:

| Display Text | Actual Destination | Risk |
|---|---|---|
| `Visit website` | `http://salpor.com/f.html` | ⚠️ Unrelated domain — possible malware/redirect |
| `Yes, I want a 5-10% discount` | `mailto:ex.pharmacy.online@gmail.com` | Collects responses on Gmail account |
| `No, I don't need Viagra/Cialis` | `mailto:ex.pharmacy.online5@gmail.com` | Confirms active email address to spammers |

- The main link points to `salpor.com`, which has **no relation** to any pharmacy brand.
- Even clicking "No" confirms your email is active — a common harvesting trick.

### 5. No Attachments — But Redirect Risk

- This email contains no direct attachment, but the link `http://salpor.com/f.html` is a redirect URL.
- Such redirect URLs are commonly used to serve **malware, fake login pages**, or **drive-by downloads**.

### 6. Generic Greeting

- The email opens with **"Hello Valued Customer"** — a generic salutation with no personalization.
- Legitimate businesses address recipients by their actual name.

### 7. Unsolicited Commercial Email (Spam/Scam)

- The email promotes **prescription medications** (Viagra, Cialis, Levitra) at suspiciously low prices.
- Selling prescription drugs via unsolicited email is **illegal** in most jurisdictions.
- The "Express Pharmacy" branding is fabricated with no verifiable business registration.

### 8. Request for Engagement / Data Harvesting

- Both "Yes" and "No" mailto links send responses to **Gmail accounts** (`ex.pharmacy.online@gmail.com`, `ex.pharmacy.online5@gmail.com`).
- Responding confirms your email address is active, leading to **more spam or targeted phishing**.

---

## Conclusion

The analyzed email (`sample-1510.eml`) exhibits **multiple confirmed phishing and spam characteristics**:

- ❌ Sender spoofing with a manipulative display name
- ❌ SPF, DKIM, and DMARC authentication all **failed**
- ❌ Suspicious redirect link to an unrelated domain (`salpor.com`)
- ❌ Email harvesting via "Yes/No" mailto links on Gmail accounts
- ❌ Generic greeting with no personalization
- ❌ Unsolicited promotion of prescription drugs — illegal in most regions
- ❌ Psychological manipulation through exclusive offer framing

Based on these indicators, this email is **confirmed to be a phishing/spam attempt**.

---

## Recommendation

> ⚠️ Take the following actions if you receive a similar email:

- **Do not** click any links, especially `http://salpor.com/f.html`.
- **Do not** reply to any mailto links — even "No" confirms your email is active.
- **Verify** sender authenticity through official channels only.
- **Report** the email as phishing/spam to your email provider.
- **Delete** the email immediately after reporting.
- **Block** the sender address `ex.pharmacy.online1@hotmail.com`.
