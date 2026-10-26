<!-- tc:header -->
<sub>[🏠 Toolcloak](../README.md) › [Docs](README.md) › **Scripts**</sub>
<!-- /tc:header -->

# Scripts

> The npm scripts and the build-time generators.

## npm scripts
| Command | What it does |
|---|---|
| `npm run dev` | Dev server at http://localhost:3000 |
| `npm run build` | `prebuild` generators, then a static export to `out/` |
| `npm start` | Next start (not needed for the static host) |
| `npm run seo` | Re-run `generate-seo-pages.js` on its own |
| `npm run og` | Rebuild social preview images (`og-data.mjs` then `make-og.py`) |
| `npm run vendor` | Re-copy vendor libraries |
| `npm test` | Fedra acceptance tests |

## `scripts/`
| File | Purpose |
|---|---|
| `copy-vendor.js` | Copy libraries from `node_modules` to `public/vendor/` |
| `generate-seo-pages.js` | Static SEO pages, sitemap, `llms.txt`, `llms-full.txt`, RSS, article Markdown, lite lists |
| `generate-tool-components.js` | Tool component map |
| `generate-tool-io.js` | What each tool takes in and gives out |
| `generate-assistant-index.js` | Fedra index, knowledge file and coverage report |
| `og-data.mjs`, `make-og.py` | Social preview images (Python with Pillow) |
| `change-domain.js` | Swap the site URL everywhere |
| `scan-secrets.js` | Look for passwords and keys in the code (also run in CI) |
| `fonts/` | Font files used by the preview-image script |


<!-- tc:footer -->
<br/>

---

<div align="center">

<sub><a href="ADDING_AN_ARTICLE.md">← Adding an Article</a> &nbsp;·&nbsp; <a href="TESTING.md">Testing →</a></sub>
<br/><br/>
<a href="../README.md"><b>← Back to README</b></a> &nbsp;·&nbsp; <a href="README.md">Documentation index</a> &nbsp;·&nbsp; <a href="https://github.com/megawiper/toolcloak.com">megawiper/toolcloak.com</a>

<br/>

<a href="../README.md"><img src="../assets/brand/logo-wordmark.png" height="44" alt="Toolcloak"/></a>

<sub>Free tools, built to be genuinely useful. Your files stay yours.</sub>

</div>
