# Security Policy

Thank you for helping keep Toolcloak and its visitors safe.

## Reporting a vulnerability

**Please do not open a public issue for security problems.**

Report privately in one of these ways:

1. **GitHub private vulnerability reporting.** Open the **Security** tab of this repository and choose **Report a vulnerability**.
2. **Contact in `security.txt`.** The contact listed at [`/.well-known/security.txt`](https://toolcloak.com/.well-known/security.txt) is the maintainer's GitHub profile. Send a message there and ask for a private channel.

Please include:
- what you found and where (URL, tool or Studio name),
- steps to reproduce,
- the impact you think it has,
- your browser and device.

## What happens next
<!-- EDIT these targets to match what you can really commit to. -->
- We aim to acknowledge your report within **3 working days**.
- We aim to share an assessment within **10 working days**.
- We will tell you when it is fixed and, if you like, credit you.

## Scope

**In scope**
- toolcloak.com: tools, Studios, Tool Chains, Fedra, guides, embeds and info pages
- Cross-site scripting and injection (for example through Docs, article rendering, share links or `#s=` data)
- Unsafe handling of uploaded files (PDF, ZIP, images, 3D, spreadsheets)
- Anything that makes a *Private* tool send your file somewhere. The "bytes uploaded" meter should stay at 0 for file uploads
- Problems with History, Artifacts, My Space, device sync or consent handling
- Ways around the sponsor-slot rules (media must be hosted on this site, `https` links only)
- Embed isolation: the rest of the site must stay frame-protected

**Out of scope**
- Automated scanner output with no demonstrated impact
- Missing best-practice headers with no exploit
- The looser Content-Security-Policy (`'unsafe-inline'`, `'unsafe-eval'`), which is a [documented trade-off](docs/SECURITY_PRACTICES.md) already on the roadmap
- Known advisories in the pinned `xlsx` library that npm cannot fix (already tracked)
- Social engineering, spam, physical attacks
- Denial-of-service and load testing
- Weaknesses in the classic ciphers we offer for learning (Caesar, ROT13, XOR and so on). They are insecure by design
- The PDF Studio black cover not removing text underneath, and signatures not being legally binding. Both are stated in the app

## Please
- Test only with your own data and devices.
- Give us reasonable time to fix before you publish.
- Do not access, change or delete other people's data.

We will not take action against people who report in good faith and follow this policy.

## Automated checks
A workflow runs a secret scan, `npm audit` and CodeQL on every push, every pull request and weekly. Details: [docs/SECURITY_PRACTICES.md](docs/SECURITY_PRACTICES.md). Privacy details: [docs/PRIVACY.md](docs/PRIVACY.md).
