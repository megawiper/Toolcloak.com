<!-- tc:header -->
<sub>[🏠 Toolcloak](../README.md) › [Docs](README.md) › **Adding an article**</sub>
<!-- /tc:header -->

# Adding an article

> Write a guide as one plain-data file.

A post is **one plain-data file** in `src/content/articles/`. No imports, so `scripts/generate-seo-pages.js` can read it.

## Steps
1. Copy any file in `src/content/articles/`.
2. Change `slug`, `tool` (an existing tool slug), `title`, `dek`, `cover.kind`, `body` and `faqs`.
3. Import it in `src/lib/articles.js` and add it to `POSTS`.
4. Run `npm run seo`, `node scripts/generate-assistant-index.js` and `npm run og`.

## Body blocks
`lead`, `p`, `h2`, `h3`, `ul`, `ol`, `quote`, `callout` (tone: tip, warn or note), `table`, `tool` (a card linking to a tool), and `fig` (`bars`, `line`, `flow`).

Inline markup in text: `**bold**`, `` `code` ``, and `[text](/tools/slug)`.

## Optional fields
`endTitle`, `endNote` (the line under the closing banner), `updated` (ISO date), and `cover: { kind, photo }` for a real photo with the drawing as fallback.

## Covers
Original vector art with no text or brand marks, drawn in `ArticleCover.jsx` with light and dark versions. Add a new `kind` there if none fits.

## Checklist
- [ ] A real question people search
- [ ] Linked to the right tool
- [ ] Real worked numbers
- [ ] FAQs written
- [ ] Not overlapping an existing guide
- [ ] `npm run seo` ran (sitemap, Markdown copy, RSS, JSON-LD)
- [ ] Fedra index regenerated


<!-- tc:footer -->
<br/>

---

<div align="center">

<sub><a href="ADDING_A_TOOL.md">← Adding a Tool</a> &nbsp;·&nbsp; <a href="SCRIPTS.md">Scripts →</a></sub>
<br/><br/>
<a href="../README.md"><b>← Back to README</b></a> &nbsp;·&nbsp; <a href="README.md">Documentation index</a> &nbsp;·&nbsp; <a href="https://github.com/megawiper/toolcloak.com">megawiper/toolcloak.com</a>

<br/>

<a href="../README.md"><img src="../assets/brand/logo-wordmark.png" height="44" alt="Toolcloak"/></a>

<sub>Free tools, built to be genuinely useful. Your files stay yours.</sub>

</div>
