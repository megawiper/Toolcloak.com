<!-- tc:header -->
<sub>[🏠 Toolcloak](../README.md) › [Docs](README.md) › **Adding a tool**</sub>
<!-- /tc:header -->

# Adding a tool

> Register it, add a route, build. The rest is automatic.

## 1. Register it
In `src/lib/tools-registry.js`:
```js
{
  slug: "your-tool-name",          // URL: /tools/your-tool-name
  name: "Your Tool Name",
  description: "Full description for SEO meta",
  shortDesc: "Short description",
  category: "Category Name",       // one of the 17 categories
  color: "#hexcolor",
  keywords: ["keyword1", "keyword2"],
}
```

## 2. Create the route
```
src/app/tools/your-tool-name/
  page.jsx            SEO metadata + ToolPageLayout wrapper
  YourToolTool.jsx    'use client', the tool UI
```

## 3. Use the kits
Build the tool with `TextTool`, `CalcTool` (in `ToolKit.jsx`) or `LabKit`. A tool built this way automatically gets History, Artifacts, the Chain builder and runner, Share link, Send to, the Wishlist star and global search.

For a hand-made tool: mark run buttons with `data-chain-run` and report results with `useChainEmit`.

## 4. Add its guide and icon
- A how-to guide entry in `src/lib/guides.js` (steps, tips, FAQ).
- An icon mapping in `ToolMark.jsx`.

## 5. Build
`npm run build` regenerates everything derived: `tool-components.js`, `tool-io.js`, `tools-lite.js`, the category pages, the footer links, `site-map`, `sitemap.xml`, `public/seo/*.html`, `llms.txt` and Fedra's index. Never edit those by hand.

## 6. Check
- Does Fedra find it by its own name? (`npm test` checks every slug.)
- Does it work in a chain? Check `ASSISTANT-REPORT.md` for gaps.
- Does it work on a phone, in light and dark?
- Does a *Private* tool really keep the input local? Watch the "bytes uploaded" pill.


<!-- tc:footer -->
<br/>

---

<div align="center">

<sub><a href="PROJECT_STRUCTURE.md">← Project Structure</a> &nbsp;·&nbsp; <a href="ADDING_AN_ARTICLE.md">Adding an Article →</a></sub>
<br/><br/>
<a href="../README.md"><b>← Back to README</b></a> &nbsp;·&nbsp; <a href="README.md">Documentation index</a> &nbsp;·&nbsp; <a href="https://github.com/megawiper/toolcloak.com">megawiper/toolcloak.com</a>

<br/>

<a href="../README.md"><img src="../assets/brand/logo-wordmark.png" height="44" alt="Toolcloak"/></a>

<sub>Free tools, built to be genuinely useful. Your files stay yours.</sub>

</div>
