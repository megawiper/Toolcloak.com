<!-- tc:header -->
<sub>[🏠 Toolcloak](../README.md) › [Docs](README.md) › **Embed a tool on your website**</sub>
<!-- /tc:header -->

# Embed a tool on your website

> Put any Toolcloak tool on your own website.

Every tool page has an **Embed this tool on your website** card a short scroll below the tool. Pick a height, copy the code.

## The hub
`/embed` lists every tool with **Copy**, **Show code** and **Preview** buttons, search and a category filter.

## What visitors see
The framed page (`/embed/<slug>`) shows **only the tool** and a plain "Powered by ToolCloak" line linking back. No header, footer, cookie banner or analytics inside the frame.

## Optional auto-height
`public/embed.js` is an optional one-line script that makes the frame fit the tool's height.

## SEO
Embedded pages are `noindex` and carry a canonical link to the real tool page.

## Security
Everything else on the site can only be framed by itself (`X-Frame-Options: SAMEORIGIN`, `frame-ancestors 'self'`). `public/_headers` gives `/embed/*` its own rule that removes `X-Frame-Options` and allows `frame-ancestors *`.

## No server
It is all static files.


<!-- tc:footer -->
<br/>

---

<div align="center">

<sub><a href="VOICE_AND_AUTOMATION.md">← Voice and Automation</a> &nbsp;·&nbsp; <a href="PWA_AND_OFFLINE.md">PWA and Offline →</a></sub>
<br/><br/>
<a href="../README.md"><b>← Back to README</b></a> &nbsp;·&nbsp; <a href="README.md">Documentation index</a> &nbsp;·&nbsp; <a href="https://github.com/megawiper/toolcloak.com">megawiper/toolcloak.com</a>

<br/>

<a href="../README.md"><img src="../assets/brand/logo-wordmark.png" height="44" alt="Toolcloak"/></a>

<sub>Free tools, built to be genuinely useful. Your files stay yours.</sub>

</div>
