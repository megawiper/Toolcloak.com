# Embed a tool on your website

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
