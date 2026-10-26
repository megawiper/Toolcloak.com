# Privacy

This page describes, in plain words, what the code does today. The legal text is on the site's Privacy Policy and Cookie Policy pages. If the code changes, update this page too.

## The short version
- Toolcloak is a **static site**. There is no server code of ours that receives your files.
- Tools run **in your browser**. The file you pick is never stored, and only a tool's result can be saved, on your device.
- History is **off until you give permission**, and it never leaves your device.
- A live counter shows **how many bytes of files this page has uploaded**. It should say 0.

## The upload meter
When you pick a file or a tool finishes, a small pill drops in from the top and says **"0 bytes uploaded"**. Tap it to open `/privacy-proof`.

How it works (`src/components/PrivacyMeter.jsx`, `src/lib/usage.js`): while the site is open, every `fetch()`, `XMLHttpRequest` and `sendBeacon()` is inspected, and the size of any **file-like body** (a File, a Blob, FormData with files, binary data, or a very large text body) is added up. The total is kept on your device only.

**Honest limit:** ordinary small text requests, such as page views and analytics, are not files and are not counted. The meter proves your *files* stay put. It is not a log of every request. For the strongest check, use a tool offline (see [PWA_AND_OFFLINE](PWA_AND_OFFLINE.md)) or watch your browser's Network tab.

## What stays on your device
| Data | Where |
|---|---|
| Files you open in a tool | Browser memory only |
| Tool results | **Artifacts** in IndexedDB, saved once at their own size |
| Studio projects | IndexedDB (My Space) |
| History (inputs, results, result files) | Local storage, only with your permission |
| Wishlist, chains, settings, theme, consent choice | Local storage |
| Fedra's chat and memory | This browser |
| Voice command log | This browser |
| Share links | Inputs sit in the URL fragment (`#s=...`), which browsers never send to a server |

## What can leave your device
| What | When | Why |
|---|---|---|
| Page, script and font requests | Always | Loading the site from Cloudflare Pages. Libraries are bundled with the site, not loaded from outside CDNs |
| **Visit counting (Google Analytics 4)** | Always for most visitors; for visitors in the EU only after pressing **Accept** | Counts visits. Advertising features and Google signals are switched off. No analytics event carries what you typed |
| Cloudflare Web Analytics | Only if a token is configured (it is empty by default) | Optional performance numbers |
| An address you type | When you use **API Request Builder, CORS Tester or DNS Lookup** | Those tools exist to reach the address you give them |
| Google's translate widget | Only when you turn translation on | Translating the page |
| Exchange rates | When your currency is not INR | Showing rupee amounts in your currency |
| Fonts (Google Fonts, gstatic) | Allowed by the security policy | Typography |

## Fedra
No language model, no server, no API. The engine files contain no `fetch`, beacon or analytics call, and a test checks that. The panel loads static files from this site. Uploaded files stay in memory. See [FEDRA](FEDRA.md).

## Sync
Device sync connects your two devices directly (WebRTC). There is no account and no server holding your data.

## Sponsors and donations
- Sponsor pictures and videos must be files on this site, so an advertiser's server never sees a visitor. Links must be `https` and every sponsor is labelled **Sponsored**. Clicking a link takes you to the sponsor's site.
- Donations use a UPI QR drawn in your browser. Payment happens in your UPI app. The supporters list is typed by hand and is not connected to UPI.
- The feedback form has sending switched off on purpose: nothing is sent to an outside service. Custom tool requests open your mail app with the text filled in. Nothing is sent from the page.

## Your controls
- **Decline cookies** and History stays off.
- **Settings:** clear history, clear wishlist, delete all artifacts, delete all chains, export or import your data, free up space.
- **Clear site data** in your browser to remove everything Toolcloak saved on this device.
- **EU visitors:** counting starts only after Accept.

## Questions
Open an issue, or see [SUPPORT](SUPPORT.md).
