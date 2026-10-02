# Web Security Policy Scanner (WSPS) 🛡️

> Scan one or hundreds of websites for common security misconfigurations — HTTP security headers, SSL/TLS certificates, HSTS and cookie flags — from a single terminal command.

![Python](https://img.shields.io/badge/python-3.8%2B-blue)
![License: MIT](https://img.shields.io/badge/license-MIT-yellow)

Born from a real pain point: validating security policies across a list of dozens of internal URLs after a deploy. What took almost an hour of manual clicking (DevTools → headers → SSL tester → HSTS preload site) now takes seconds.

📖 **Read the story behind this tool:** [Como Automatizei uma Verificação de Segurança Web com um Script Python](https://www.silvasec.seg.br/como-automatizei-uma-simples-verifica%C3%A7%C3%A3o-de-seguran%C3%A7a-web-com-um-script-python-8d2ddeb2f062)

---

## What it checks

| Category | Checks |
|---|---|
| **Transport** | HTTP → HTTPS redirect · TLS certificate hostname match · certificate validity ≤ 398 days |
| **HSTS** | `Strict-Transport-Security` header · `includeSubDomains` · presence on the [HSTS preload list](https://hstspreload.org) (via official API) |
| **CSP** | `Content-Security-Policy` presence · flags `unsafe-eval` and `unsafe-inline` |
| **Cookies** | `Secure` and `HttpOnly` attributes on all cookies |
| **Info disclosure** | `Server` header exposure |
| **Headers** | `X-Content-Type-Options: nosniff` · `X-Frame-Options: DENY/SAMEORIGIN` |

Results are printed as a color-coded `[PASS]` / `[FAIL]` / `[WARN]` / `[INFO]` report per URL.

---

## Installation

Requires **Python 3.8+**.

```bash
git clone https://github.com/Silva-Sec/WebSecurityPolicyScanner.git
cd WebSecurityPolicyScanner
pip install -r requirements.txt
```

---

## Usage

**Scan a single domain:**

```bash
python wsps.py github.com
```

**Scan a list of URLs (one per line):**

```bash
python wsps.py urls_to_validate.txt
```

Example input file:

```
github.com
https://example.org/login
internal-app.corp.local
```

---

## Example output

```
==================================================
Analyzing: github.com
==================================================
[INFO] Checking HTTP -> HTTPS redirect...
[PASS] HTTP correctly redirects to HTTPS.
[INFO] Checking SSL certificate...
[PASS] Hostname 'github.com' matches the SSL certificate.
[PASS] SSL validity period is 365 days (within the 398-day limit).
[INFO] Checking HSTS Preload List...
[PASS] Domain status is 'preloaded' on the HSTS Preload List.
[INFO] Analyzing security headers...
[PASS] CSP is implemented.
[PASS] CSP does not contain 'unsafe-eval'.
...
```

---

## Roadmap

- [ ] Security score (A–F grade per URL)
- [ ] TLS version detection (flag TLS 1.0/1.1)
- [ ] SameSite cookie attribute check
- [ ] Modern headers: COOP, CORP, COEP, Referrer-Policy, Permissions-Policy
- [ ] CSV/JSON report export for batch audits

Contributions are welcome — open an [Issue](../../issues) or a Pull Request.

---

## Author

**Jonathan Silva** — Security Researcher & Infrastructure Specialist.

- 🌐 [silvasec.com](https://silvasec.com)
- ✍️ [silvasec.seg.br](https://www.silvasec.seg.br)
- 🐦 [@SilvaSec](https://x.com/SilvaSec)

## License

[MIT](LICENSE) © 2026 Jonathan Silva
