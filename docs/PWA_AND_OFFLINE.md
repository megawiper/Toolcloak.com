<!-- tc:header -->
<sub>[🏠 Toolcloak](../README.md) › [Docs](README.md) › **Install and offline**</sub>
<!-- /tc:header -->

# Install and offline

> Install it like an app. Keep working without a connection.

Toolcloak is a progressive web app.

| Piece | File |
|---|---|
| Service worker | `public/sw.js` |
| Manifest | `public/manifest.webmanifest` |
| Offline page | `public/offline.html` |
| Registration and install card | `src/components/PwaRegister.jsx` |

## What works offline
- Pages a visitor has already opened.
- Libraries such as `pdf-lib`, cached with the pages.
- Your saved projects, history and results, which are on the device anyway.

## Installing
After about 25 seconds on the site an **Install app** card is offered.

## Updating everyone's cache
Bump `VERSION` in `public/sw.js`. Settings also has a **Check for updates** button for visitors.

## Offline and privacy
Because the tools run in the browser, a tool you have opened before keeps working with no connection at all. That is also a handy way to prove nothing is being uploaded: turn off your network and use the tool.


<!-- tc:footer -->
<br/>

---

<div align="center">

<sub><a href="EMBED.md">← Embed</a> &nbsp;·&nbsp; <a href="REGION_AND_TRANSLATE.md">Region and Translate →</a></sub>
<br/><br/>
<a href="../README.md"><b>← Back to README</b></a> &nbsp;·&nbsp; <a href="README.md">Documentation index</a> &nbsp;·&nbsp; <a href="https://github.com/megawiper/toolcloak.com">megawiper/toolcloak.com</a>

<br/>

<a href="../README.md"><img src="../assets/brand/logo-wordmark.png" height="44" alt="Toolcloak"/></a>

<sub>Free tools, built to be genuinely useful. Your files stay yours.</sub>

</div>
