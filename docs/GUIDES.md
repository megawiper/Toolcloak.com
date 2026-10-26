# Guides (articles)

188 long-form guides at [toolcloak.com/articles](https://toolcloak.com/articles). Each starts with a tool you already use, then shows what happens under the hood with real numbers.

## How the library works
- `/articles` is the index (with numbered pages and its own search). `/articles/<slug>` is one guide.
- **Every guide is tied to one tool.** The tool page shows a *Full guide* card, and the guide links back with one button at the top and one small *Open tool* link inside.
- Every tool page ends with four square guide tiles (same tool first, then same category, then the rest). Every guide ends with the same kind of tiles.
- The home page shows the latest four guides. The menu and footer link to them.
- **Search** looks through every section of every guide and links to the matching heading. The main site search shows matching sections as its last group.
- Guides are written around real search questions, often with 7 or 8 FAQs (some with a Hinglish question such as "pdf ka password kaise hataye") and written from what the tool really does today, limits included.

## Look and feel
- Original vector covers with no text or brand marks, in light and dark versions.
- Animated bar charts and a line chart you can touch to read values, plus flow and gauge figures.
- A dedicated dark theme. Articles keep their own colours (`data-keep-light`).
- Smaller type, a poster under the first paragraph, `content-visibility: auto` on big blocks.
- A real photo can replace a cover (`cover.photo`), with the drawing as fallback.

## Discovery
`npm run seo` (also run before every build) writes, for each guide: a static HTML page with BlogPosting, FAQ and Breadcrumb JSON-LD, a clean Markdown copy in `public/articles/`, an entry in `llms-full.txt`, an RSS item with full text, and a sitemap entry with its own `lastmod`. See [SEO](SEO.md).

## Writing one
See [ADDING_AN_ARTICLE](ADDING_AN_ARTICLE.md).

## Quality checklist
1. Start from a real question people type.
2. Link the guide to the tool it explains.
3. Use real worked numbers.
4. Keep the title specific.
5. **Do not overlap an existing guide.** Two guides on the same question compete in search. (The colour-format guides `hex-rgb-and-hsl-three-ways-to-write-the-same-colour` and the same name ending `-and-which-to-use` are close. Merge or differentiate them and redirect the old URL.)
