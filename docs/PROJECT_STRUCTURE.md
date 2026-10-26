<!-- tc:header -->
<sub>[🏠 Toolcloak](../README.md) › [Docs](README.md) › **Project structure**</sub>
<!-- /tc:header -->

# Project structure

> Where everything lives in the repository.

```
.
├── .github/workflows/      security.yml (secret scan, npm audit, CodeQL), lockfile.yml
├── public/
│   ├── _headers            security headers + caching rules (Cloudflare Pages)
│   ├── _redirects          /blog -> /articles
│   ├── .well-known/        security.txt
│   ├── articles/           one clean Markdown copy per guide (188)
│   ├── assistant/          Fedra index, knowledge and report (generated)
│   ├── seo/                static SEO pages (generated)
│   ├── icons/ og/          app icons, social preview images
│   ├── sponsors/           ad pictures and videos
│   ├── templates/          template-pack assets
│   ├── sw.js               service worker
│   ├── manifest.webmanifest, offline.html, embed.js
│   └── sitemap.xml, robots.txt, llms.txt, llms-full.txt, feed.xml
├── scripts/                build-time generators and helpers
├── src/
│   ├── app/                routes (App Router)
│   ├── components/         UI, tool kits, studios
│   ├── content/articles/   one plain-data file per guide (188)
│   └── lib/                engines and data (95 modules)
├── tests/                  17 Node test files + helpers
├── next.config.js, tailwind.config.js, postcss.config.js, jsconfig.json
└── package.json
```

## Routes (`src/app`)
| Route | What it is |
|---|---|
| `/` | Home: hero, search, Drop Zone box, popular tools, latest guides |
| `/tools/<slug>` | One page per tool (584) |
| `/tools` | All tools, grouped by category |
| `/articles`, `/articles/<slug>` | Guides |
| `/studio`, `/studio/pdf`, `/studio/design`, `/studio/data`, `/studio/geometry`, `/studio/3d` | Studios |
| `/office/docs`, `/office/sheets` | Docs and Sheets studios |
| `/drop` | Drop Zone |
| `/chains`, `/chains/run`, `/chains/templates/<id>` | Tool Chains |
| `/library` | My Space |
| `/artifacts`, `/history`, `/wishlist` | Saved results, history, starred tools |
| `/assistant` | Fedra full page |
| `/automation` | Voice command log |
| `/sync` | Device-to-device sync |
| `/privacy-proof` | The page behind the "0 bytes uploaded" pill |
| `/settings` | Region, language, voice, storage, backup |
| `/embed`, `/embed/<slug>` | Embed hub and framed tools |
| `/donate`, `/supporters`, `/sponsor`, `/templates`, `/custom-tool` | Support and business pages |
| `/about`, `/contact`, `/privacy-policy`, `/terms-of-service`, `/disclaimer`, `/cookie-policy`, `/faq`, `/how-it-works`, `/site-map` | Information pages |

## Generated files (do not edit by hand)
`src/lib/tool-components.js`, `tool-io.js`, `tools-lite.js`, `home-lite.js`, `categories-lite.js`, everything in `public/seo/`, `public/assistant/`, `sitemap.xml`, `llms.txt`, `llms-full.txt`, `feed.xml`, `public/articles/*.md`, `ASSISTANT-REPORT.md`.

## Where the interesting code lives
| Area | Look here |
|---|---|
| Tool registry | `src/lib/tools-registry.js` |
| Shared tool kits | `src/components/ToolKit.jsx`, `LabKit.jsx`, `BizUi.jsx` |
| Chains | `src/lib/chains.js`, `chainRules.js`, `src/components/ChainRunner.jsx`, `ChainEditor.jsx` |
| Fedra | `src/lib/assistant/*.mjs`, `src/components/assistant/` |
| PDF Studio | `src/components/studio/PdfStudio.jsx`, `src/lib/pdfOps.js`, `pdfMarkup.js` |
| Privacy meter | `src/components/PrivacyMeter.jsx`, `PrivacyProof.jsx`, `src/lib/usage.js` |
| Sync | `src/lib/syncRtc.js`, `syncTransfer.js`, `src/components/SyncClient.jsx` |
| Voice | `src/components/VoiceCommand.jsx`, `src/lib/voiceAgent.js` |
| Business settings | `src/lib/business.js`, `donate.js`, `sponsors.js`, `supporters.js` |


<!-- tc:footer -->
<br/>

---

<div align="center">

<sub><a href="TECH_STACK.md">← Tech Stack</a> &nbsp;·&nbsp; <a href="ADDING_A_TOOL.md">Adding a Tool →</a></sub>
<br/><br/>
<a href="../README.md"><b>← Back to README</b></a> &nbsp;·&nbsp; <a href="README.md">Documentation index</a> &nbsp;·&nbsp; <a href="https://github.com/megawiper/toolcloak.com">megawiper/toolcloak.com</a>

<br/>

<a href="../README.md"><img src="../assets/brand/logo-wordmark.png" height="44" alt="Toolcloak"/></a>

<sub>Free tools, built to be genuinely useful. Your files stay yours.</sub>

</div>
