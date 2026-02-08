

```markdown
<p align="center">
  <pre>
 ____  _ _     ____
/ ___|(_) | __/ ___|  ___ __ _ _ __
\___ \| | |/ /\___ \ / __/ _` | '_ \
 ___) | |   <  ___) | (_| (_| | | | |
|____/|_|_|\_|____/ \___\__,_|_| |_|
  </pre>
</p>

<h1 align="center">SikScan v2.2</h1>
<p align="center"><b>E-Commerce Intelligence Scanner</b></p>

<p align="center">
  <img src="https://img.shields.io/badge/version-2.2-blue?style=flat-square" />
  <img src="https://img.shields.io/badge/platform-Linux%20%7C%20macOS-lightgrey?style=flat-square" />
  <img src="https://img.shields.io/badge/dev-%40skittlehideout-purple?style=flat-square" />
</p>

---

## 📖 What Is SikScan?

SikScan is a **high-speed reconnaissance tool** designed for scanning e-commerce websites at scale. It identifies:

- **🛒 CMS / Platform** — What the site is built on (Shopify, WooCommerce, Magento, BigCommerce, PrestaShop, Wix, Squarespace, etc.)
- **💳 Payment Gateways** — What payment processors the site uses (Stripe, PayPal, Braintree, Square, Klarna, Afterpay, Razorpay, Authorize.net, and 20+ more)
- **🛡️ Security Posture** — Whether the site is protected by CAPTCHAs (reCAPTCHA, hCaptcha, Turnstile) or WAFs (Cloudflare, Akamai, Sucuri, Imperva, etc.)

Each target is classified into one of three categories:

| Status | Meaning |
| :--- | :--- |
| 🟢 **CLEAN** | Site is accessible, has e-commerce input fields, no blocking detected |
| 🔴 **HARD** | Site is protected by CAPTCHA, WAF, or returned a block status |
| ⚫ **DEAD** | Site is unreachable, timed out, or returned a server error |

---

## ⚡ Features

- **Massively concurrent** — Scan hundreds of targets simultaneously
- **Deep payment detection** — Doesn't just check the landing page; intelligently probes checkout flows, APIs, and cart pages to uncover payment gateways that aren't visible on the surface
- **30+ payment gateway detection** — Stripe, PayPal, Braintree, Square, Adyen, Klarna, Afterpay, Affirm, Sezzle, Razorpay, PayU, Authorize.net, 2Checkout, WorldPay, SagePay, and more
- **15+ CMS detection** — Shopify, WooCommerce, WordPress, Magento, BigCommerce, PrestaShop, Wix, Squarespace, Drupal, Joomla, and more
- **WAF & CAPTCHA identification** — Cloudflare, Akamai, Imperva, Sucuri, Datadome, reCAPTCHA, hCaptcha, Turnstile, Arkose Labs, and more
- **Smart category page handling** — Automatically follows product links from category/collection pages
- **Isolated sessions** — Each target gets its own session; no cross-contamination
- **Organized output** — Results sorted into `clean_auto.txt`, `blocked_hard.txt`, and `dead.txt` with timestamps
- **Color-coded terminal output** — Green for clean, red for hard, gray for dead

---

## 📦 Installation

### Prerequisites
- Linux or macOS (x86_64)
- Terminal with ANSI color support

### Download & Setup

```bash
# Clone the repo
git clone [https://github.com/YOUR_USERNAME/sikscan.git](https://github.com/YOUR_USERNAME/sikscan.git)
cd sikscan

# Make executable
chmod +x sikscan start.sh

```

That's it. No dependencies, no runtime, no installation. Single binary.

---

## 🚀 Usage

### Quick Start (Recommended)

```bash
./start.sh

```

The interactive launcher will:

1. Detect all `.txt` files in the current directory
2. Let you select your targets file with arrow keys
3. Launch the scan automatically

### Direct Usage

```bash
./sikscan -f targets.txt

```

### All Options

```text
Usage: ./sikscan [options]

Options:
  -f string     Targets file (default "targets.txt")
  -w int        Number of concurrent workers (default 20)
  -t int        HTTP timeout in seconds (default 12)
  -d int        Delay between requests per worker in ms (default 0)
  -deep         Enable deep payment detection (default true)

```

### Examples

```bash
# Basic scan with defaults
./sikscan -f my_targets.txt

# Fast scan with 50 workers
./sikscan -f targets.txt -w 50

# Careful scan with delays (avoid rate limiting)
./sikscan -f targets.txt -w 10 -d 500

# Quick surface scan without deep probing
./sikscan -f targets.txt -deep=false

# Long timeout for slow targets
./sikscan -f targets.txt -t 30

```

---

## 📝 Targets File Format

Create a `.txt` file with one URL per line:

```text
[https://example-store.com/shop/](https://example-store.com/shop/)
[https://another-store.com/product-category/shoes/](https://another-store.com/product-category/shoes/)
[https://myshop.com/collections/all](https://myshop.com/collections/all)
[https://store.example.org/](https://store.example.org/)

```

* URLs are automatically deduplicated
* HTTP URLs are accepted (no need to manually convert to HTTPS)
* Query parameters and paths are preserved
* Invalid or malformed URLs are silently skipped

---

## 📊 Output

### Terminal (Real-time)

```text
[CLEAN] [https://example.com/shop/](https://example.com/shop/) | CMS: WooCommerce, WordPress | PAY: Stripe, PayPal | REASON: Found: E-commerce Input Fields
[HARD]  [https://blocked.com/store/](https://blocked.com/store/) | CMS: WordPress | PAY: Stripe | REASON: Blocked: reCAPTCHA detected
[DEAD]  [https://offline.com/shop/](https://offline.com/shop/) | CMS: - | PAY: - | REASON: Error: connection refused

```

### Files

Results are saved to a timestamped folder:

```text
results/
└── 20260208_143022/
    ├── clean_auto.txt      ← Accessible sites with e-commerce fields
    ├── blocked_hard.txt    ← Protected sites (CAPTCHA/WAF/blocked)
    └── dead.txt            ← Unreachable or errored sites

```

Each line in the output files contains:

```text
[STATUS] URL | CMS: detected_cms | PAY: detected_payments | REASON: classification_reason | CODE: http_status

```

### Summary

After every scan, a summary is displayed:

```text
══════════════════════════════════════════
  Results saved to: results/20260208_143022
  CLEAN: 25 | HARD: 27 | DEAD: 2 | Total: 54
══════════════════════════════════════════

```

---

## 🔍 What Each Status Means

### 🟢 CLEAN

The site responded successfully (HTTP 200), contains e-commerce input fields (cart forms, checkout fields, product buttons), and **no CAPTCHA or WAF protection was detected**. These sites are directly interactable.

### 🔴 HARD

The site is **protected** by one or more of the following:

* CAPTCHA challenge (reCAPTCHA, hCaptcha, Cloudflare Turnstile, etc.)
* Web Application Firewall (Cloudflare, Akamai, Imperva, Sucuri, etc.)
* HTTP 403 Forbidden response
* The site loads but has no actionable e-commerce fields

### ⚫ DEAD

The site is **unreachable**:

* Connection timeout
* DNS resolution failure
* HTTP 404 Not Found
* HTTP 5xx Server Error
* SSL/TLS errors

---

## 💳 Detected Payment Gateways

| Gateway | Variants Detected |
| --- | --- |
| **Stripe** | Stripe.js, Stripe Elements, WC Payments, UPE |
| **PayPal** | Standard, PPCP, Braintree-powered, SDK |
| **Braintree** | Hosted Fields, Drop-in UI |
| **Square** | Square Payments, WooCommerce Square |
| **Authorize.net** | Accept.js, CIM Gateway |
| **Klarna** | Klarna Payments, Klarna Checkout |
| **Afterpay** | Afterpay Gateway |
| **Affirm** | Affirm.js |
| **Sezzle** | Sezzle Checkout |
| **Razorpay** | Razorpay Checkout |
| **PayU** | PayU Gateway |
| **Adyen** | Adyen Web Components |
| **2Checkout** | 2Checkout Inline |
| **WorldPay** | WorldPay Gateway |
| **SagePay/Opayo** | SagePay Direct |

## 🛒 Detected Platforms

| Platform | Detection Method |
| --- | --- |
| **Shopify** | CDN, meta tags, payment config |
| **WooCommerce** | Plugin paths, gateway classes, REST API |
| **WordPress** | wp-content, wp-includes |
| **Magento** | Frontend markers, REST API |
| **BigCommerce** | CDN markers |
| **PrestaShop** | Module paths |
| **Wix** | Static assets |
| **Squarespace** | Auth tokens, static assets |
| **Drupal** | Settings, file paths |
| **Joomla** | Media paths |
| **Salesforce** | Demandware markers |

## 🛡️ Detected Security

| Type | Solutions Detected |
| --- | --- |
| **CAPTCHA** | Google reCAPTCHA, hCaptcha, Cloudflare Turnstile, Geetest, Arkose Labs (FunCaptcha), AWS WAF CAPTCHA |
| **WAF** | Cloudflare, Akamai, Imperva/Incapsula, Sucuri, AWS WAF, Datadome, DDoS-Guard, PerimeterX, Distil Networks, ShieldSquare |

---

## ⚠️ Disclaimer

This tool is provided for **educational and authorized security research purposes only**.

* Only scan websites you **own** or have **explicit written permission** to test.
* Unauthorized scanning may violate computer fraud and abuse laws in your jurisdiction.
* The developer assumes **no liability** for misuse of this tool.
* Respect `robots.txt` and rate limits.
* Use the `-d` flag to add delays between requests when scanning sites you are authorized to test.

---

## 📬 Contact

**Developer:** @skittlehideout
**Version:** 2.2

<p align="center">
<sub>Built with ☕ and questionable life decisions</sub>
</p>

```

```
