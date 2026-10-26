# SEO and discovery

## What is in the sitemap
`public/sitemap.xml` (generated) lists **812 URLs**:
- 584 tool pages
- 188 guides, each with its own `lastmod`
- 40 other pages: home, `/articles`, `/studio` and the studio routes, `/drop`, `/chains` and its 12 templates, `/about`, `/contact`, `/privacy-policy`, `/terms-of-service`, `/disclaimer`, `/cookie-policy`, `/faq`, `/how-it-works`, `/site-map`, `/privacy-proof`, `/embed`, `/donate`, `/supporters`, `/sponsor`, `/templates`, `/custom-tool`

## Generated for crawlers (`npm run seo`, also before every build)
| File | What |
|---|---|
| `public/seo/*.html` | A static HTML page per tool, category, studio and guide, with the full text and JSON-LD, so search engines get content without running JavaScript |
| `public/seo/articles/<slug>.html` | Full guide text with **BlogPosting, FAQ and Breadcrumb** JSON-LD |
| `public/articles/<slug>.md` | The guide as clean Markdown (linked with `rel="alternate"`) |
| `public/llms.txt`, `public/llms-full.txt` | A map of the site, and every guide in one file, for AI crawlers |
| `public/feed.xml` | RSS with full text |
| `public/robots.txt` | Allows search and AI crawlers |
| `public/sitemap.xml` | See above |
| `public/og/*` | Social preview images (`npm run og`) |
| `public/BingSiteAuth.xml` | Bing Webmaster verification |

## On every page
Individual metadata, Open Graph tags, keywords, a visible trail bar (the old "Home > ..." breadcrumb stays in the HTML for search engines but is hidden visually), FAQ sections for rich results, internal links between tools and guides, and a list of guides on tool, category and home static pages.

## Site name and favicon in Google
We want results to show **Toolcloak** (not `toolcloak.com`) and the Toolcloak icon.
- Add `WebSite` structured data on the homepage with `name` and `alternateName`.
- Keep one site name in the title, `og:site_name` and the structured data.
- Serve a square favicon that is a multiple of 48 px (for example 192x192) at a stable URL. `assets/brand/logo-icon.png` is a good source. Files already in `public/`: `favicon.ico`, `favicon.png`, `favicon-dark.ico`.
- Do not block the homepage or the favicon in `robots.txt`.
- Allow days to weeks for Google to refresh.

## Housekeeping checklist
- [ ] One unique title, description and canonical per page
- [ ] **No two guides on the same question.** The two colour-format guides (`hex-rgb-and-hsl-three-ways-to-write-the-same-colour` and the one ending `-and-which-to-use`) overlap. Merge or differentiate them, and redirect the old URL in `public/_redirects`
- [ ] Update `lastmod` only when a page really changes
- [ ] Submit `sitemap.xml` in Google Search Console and Bing Webmaster Tools
- [ ] Watch Coverage and Core Web Vitals
- [ ] Run `npm run seo` and `npm run og` after adding a guide

## Writing for search
Target questions people really type ("compress image to 50 KB", "CGPA to percentage", "pdf ka password kaise hataye"). Answer in the first paragraph, then go deep with worked numbers. Write from what the tool really does, limits included.
