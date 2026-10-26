<!-- tc:header -->
<sub>[🏠 Toolcloak](../README.md) › [Docs](README.md) › **Security practices**</sub>
<!-- /tc:header -->

# Security practices

> How security works here, and where the honest trade-offs are.

How security works at Toolcloak, and where the honest trade-offs are. To report a problem, read [../SECURITY.md](../SECURITY.md).

## Principles
1. **Never receive the file.** A static site has no server of ours to leak from.
2. **Keep everything on one origin.** Libraries are bundled with the site.
3. **Collect as little as possible.** History is opt-in and local.
4. **Say what is not secure.** Classic ciphers are for learning. Black cover is not redaction. We write the limits down.
5. **Check automatically.** Do not rely on remembering.

## Headers (`public/_headers`)
Applied to every page:

| Header | Value |
|---|---|
| `Content-Security-Policy` | Scripts from this site plus Google Analytics, Cloudflare Insights and Google Translate. `object-src`, `base-uri`, `form-action` and `frame-ancestors` locked to this site. `upgrade-insecure-requests` |
| `X-Content-Type-Options` | `nosniff` |
| `X-Frame-Options` | `SAMEORIGIN` |
| `Referrer-Policy` | `strict-origin-when-cross-origin` |
| `Strict-Transport-Security` | `max-age=31536000; includeSubDomains` |
| `Permissions-Policy` | camera, microphone, display-capture and fullscreen for this site only. Geolocation, payment, USB, Bluetooth, serial and interest-cohort off |
| `Cross-Origin-Opener-Policy` | `same-origin-allow-popups` |

`/embed/*` has its own rule that removes `X-Frame-Options` and allows framing anywhere, so tools can be embedded. The rest of the site stays frame-protected.

### Trade-offs we know about
- **`'unsafe-inline'` and `'unsafe-eval'` are in `script-src`.** The framework and some bundled libraries need them today. Tightening this (nonces or hashes) is on the [roadmap](ROADMAP.md).
- **`connect-src` allows `https:`** because API Request Builder, CORS Tester and DNS Lookup must reach the address the visitor types.
- **`img-src` allows `https:`** for sponsor and external images.
If something ever breaks after a change, open the browser console: a blocked item is named there. For a quick test, rename the header to `Content-Security-Policy-Report-Only`.

## Automated checks (`.github/workflows/security.yml`)
Run on every push, every pull request and **weekly** (Monday 03:17 UTC), and on demand:

| Job | What it does |
|---|---|
| **Secrets** | `scripts/scan-secrets.js` looks for passwords and keys in the code |
| **npm audit** | Reports known problems in libraries. Report-only: known unfixable items such as `xlsx` are listed, not blocking |
| **CodeQL** | Code scanning for JavaScript and TypeScript (needs a public repo or GitHub code scanning; otherwise it shows a grey warning) |

`.github/workflows/lockfile.yml` creates `package-lock.json` through a pull request so builds install exactly the versions that were checked.

## `/.well-known/security.txt`
Lists a contact, preferred languages (English, Hindi), the canonical URL and an expiry date. **Renew the expiry before it passes.**

## Code-level defences
- **Docs** sanitises all HTML.
- **Fedra's calculator** is a safe parser with no `eval`.
- **Sponsor rules are enforced in code and tested:** media must be a file on this site, links must be `https`, every sponsor is labelled "Sponsored".
- **Embed pages** carry no header, footer, cookie banner or analytics, and are `noindex`.
- **Share links** keep inputs in the URL fragment.
- **File parsers** (PDF, ZIP, images, 3D, spreadsheets) handle untrusted input, so errors are caught and reported as friendly messages.

## What our security tools are and are not
| Tool | Note |
|---|---|
| Password generator and strength checker | A strong password still needs to be unique per site |
| Hash generator | Hashing is not encryption and cannot be reversed |
| AES text and file encryptors | Your passphrase is the key. Lose it and the data is gone |
| Classic ciphers (Caesar, Vigenere, ROT13, XOR and others) | For learning and puzzles. **Not secure** |
| Base64, Base32, Base58 | Encodings, not encryption |
| JWT decoder | Decoding is not verifying |
| PDF Studio black cover | Hides what you see. The text stays in the file. Use *Shrink hard* to remove content |
| PDF Studio signature | A picture, not a legal digital signature |

## Known gaps
- No committed `package-lock.json` yet (the workflow to create one exists).
- `xlsx` 0.18.5 is pinned and has known advisories that cannot be fixed by upgrading from npm. Treat spreadsheet files as untrusted input.
- The CSP is looser than ideal (see above).
- No `Dependabot` configuration is committed.

## Checklist for contributors
- [ ] No third-party script receives user input
- [ ] No tool input or result is logged or sent anywhere
- [ ] New dependencies are reviewed, and vendored through `copy-vendor.js`
- [ ] Anything rendered as HTML is sanitised
- [ ] File parsers catch errors and never crash the page
- [ ] No secrets, keys or tokens in commits (the scanner will tell you)
- [ ] A change to `_headers` was tested with the browser console open
- [ ] Network-dependent tools say so on their page

## Reporting
Please report privately. See [../SECURITY.md](../SECURITY.md).


<!-- tc:footer -->
<br/>

---

<div align="center">

<sub><a href="PRIVACY.md">← Privacy</a> &nbsp;·&nbsp; <a href="FAQ.md">FAQ →</a></sub>
<br/><br/>
<a href="../README.md"><b>← Back to README</b></a> &nbsp;·&nbsp; <a href="README.md">Documentation index</a> &nbsp;·&nbsp; <a href="https://github.com/megawiper/toolcloak.com">megawiper/toolcloak.com</a>

<br/>

<a href="../README.md"><img src="../assets/brand/logo-wordmark.png" height="44" alt="Toolcloak"/></a>

<sub>Free tools, built to be genuinely useful. Your files stay yours.</sub>

</div>
