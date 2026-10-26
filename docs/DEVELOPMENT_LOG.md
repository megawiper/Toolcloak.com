> **Archive.** This is the original development log that used to be the repository README. It is kept as history: version notes, design decisions and file-level details. The current README is in the repository root.

# Toolcloak — Free Online Tools Website

A production-ready Next.js tools website with auto-registering tool registry, full SEO, and 299+ working tools.

## Tool Chains (reworked)

- `/chains`: My chains, removable starter ideas, a "How to build a chain" guide. New chains show a progress bar (saving, loading each tool, checking links) and open at 100%.
- Each tool is tagged with what it takes in and gives out (`scripts/generate-tool-io.js` builds `src/lib/tool-io.js` before every build). The builder and runner use it to flag steps that cannot connect before anything runs, and offer a one-tap fix (e.g. add PDF to Word).
- `/chains/run`: Pause, Pause after every step, Pause before a step, open any step to see what went in and out, edit text or swap a file, change settings, then Continue from where it stopped. Errors stop the chain at that step with the reason; Retry / Skip / Edit.
- Tools made with ToolKit and LabKit now report results to the chain directly (`useChainEmit`); downloads are captured while a chain runs.


## Tool Chains, round 2

- **Run / Pause bar** now sits on the page under the last tool (no longer stuck to the bottom of the screen).
- **Preview frames**: pictures, the first page of a PDF (pdf.js), audio and video players, shown framed under the upload box, in each step's result and in the final result (`ChainPreview.jsx`).
- **Do this step myself** (`ManualSheet.jsx`): when a step fails, or from any step, the real tool opens with the step's input and settings already in it. Its result (or a file downloaded from it) is caught and put into the chain with one tap; you can also give your own file or text. Later steps are cleared so Continue runs the rest.
- **Settings while building and before running**: gear on every step in the builder and on the chain page (`SettingsSheet.jsx`). Chosen options show as small chips on the step.
- **Fit check as you add**: in the builder every search result is marked Fits / Cannot follow for the last step; picking one that cannot follow asks first and can add the missing converter for you (`canFollow` in `chainRules.js`).
- **Confirm risky steps**: watermark, page numbers, sign, delete/extract pages, crop, blur, metadata remover and encrypt ask before they are added and again before the chain runs. A second step of the same kind (for example two watermarks) gets a stronger warning (`riskNotes`).
- **Chain button on every tool page** (`AddToChain.jsx`): add the tool to a chain you have, or start a new chain with it (`/chains?new=slug`).
- **Hand-made tools now work inside chains**: run buttons marked with `data-chain-run`, results reported with `useChainEmit`, and the runner no longer clicks mode tabs or preset buttons by mistake.
- Extras: failed steps are retried once automatically (switch on the chain page), Duplicate chain on `/chains`.

## Results, Artifacts and History (rework)

- Every result a tool makes is stored in Artifacts automatically (IndexedDB), whether or not Download is pressed. Only the result is stored, with exactly its own size. The file you upload is never stored, and a copy of an upload is skipped. The same result is stored once.
- Next to each result: Download, Send to, and **Save to Artifacts** (it turns into **Saved** when it is stored). Tools that do not use the shared result card get a small "Result file" panel under the tool (`ResultDock.jsx`) with the same Download / Save to Artifacts / Saved states.
- Long conversions: results are caught when the file appears (`onNewBlob` in `lib/outputCapture.js`), not only after a quiet moment, so slow tools are saved too.
- History records the picked file name (the file box is emptied after a pick, so the name is kept in `data-names`), keeps the result, works for tools without fields, and updates one entry for one run.
- No emoji in the interface. Tick and cross marks inside pass/fail checklists, plain arrows (-> <-> etc.) and the emoji / symbol picker tools are left as they are.

## Article (v9)

- `/articles` (index) and `/articles/<slug>` (one page per post). Every post is tied to one tool: the tool page shows a "Full guide" card under the tool, and the post links back to the tool with one button at the top and one small plain "Open tool" link inside the article. The home page shows the latest four posts as small cards, the menu lists every post at the bottom, the footer links to the articles, and the sitemap / `llms.txt` list every post.
- **v16 additions.** Four more posts (GST, JSON, colour contrast, Base64). Every tool page now ends with four small square guide tiles of the same type (`RelatedGuides` in `ArticleCard.jsx`, picked by `lib/articlesRelated.js`: same tool, then same category, then the rest), and every post ends with the same kind of tiles. Posts show Home › Article › title at the top.
- **Article SEO and AI discovery.** `npm run seo` (also runs before build) now writes: `public/seo/articles/<slug>.html` and `public/seo/articles/index.html` (full article text, BlogPosting + FAQ + Breadcrumb JSON-LD), `public/articles/<slug>.md` (the article as clean Markdown, linked from each page with `rel=alternate`), `public/llms-full.txt` (every article in one file), `public/feed.xml` (RSS with full text), plus article entries in `llms.txt`, per-post `lastmod` in the sitemap, more AI crawlers allowed in `robots.txt`, and a "guides" list on every tool, category and home static page. Put photos in `public/articles/` if you like: only the generated `.md` files are cleared there.
- Article search: the articles page has its own search box (`ArticleSearch.jsx`, `lib/articlesFind.js`) that looks through every section of every post and links to the matching heading. The main site search (menu and home page) shows matching article sections as the last group under all other results (`lib/articlesIndex.js` builds the sections).
- A post is one plain-data file in `src/content/articles/` (no imports, so `scripts/generate-seo-pages.js` can read it). To add one: copy any file there, change `slug`, `tool` (an existing tool slug), `title`, `dek`, `cover.kind`, `body` and `faqs`, then import it in `src/lib/articles.js` and add it to `POSTS`.
- Body blocks: `lead`, `p`, `h2`, `h3`, `ul`, `ol`, `quote`, `callout` (tone tip / warn / note), `table`, `tool` (card linking to a tool), and `fig` (`bars`, `line`, `flow`). Inline markup inside text: `**bold**`, `` `code` ``, `[text](/tools/slug)`.
- Covers are original vector art with no text or brand marks (`ArticleCover.jsx`, kinds: compress, loan, growth, lock, qr, pdf, words, keys, tax, braces, contrast, encode) and have light and dark versions. Graphs are in `ArticleCharts.jsx` (animated bars, a line chart you can touch to read values). The articles has its own dark theme in `articleCss.js` (every article root carries `data-keep-light`, so the global dark engine leaves it alone). For a real photo, put it in `public/articles/` and set `cover: { kind: '...', photo: '/articles/name.jpg' }`; the drawing stays as the fallback.
- Optional per-post fields: `endTitle`, `endNote` (the line under the closing banner), `updated` (ISO date).
- **Article phase 1 (5 high-search guides).** `pdf-compress` (PDF Compressor), `image-to-kb` (Compress Image to Exact KB), `jpg-to-pdf` (Image to PDF), `heic-to-jpg` (HEIC to JPG), `split-pdf` (Split PDF). Each is a long how-to written around the questions people actually search (7-8 FAQs, tables, flow/gauge/bars figures, links to related tools and older articles). Five new original covers were added to `ArticleCover.jsx` (`pdfshrink`, `kbtarget`, `imgtopdf`, `heicjpg`, `splitpdf`). After adding, `npm run seo`, `node scripts/generate-assistant-index.js` and `npm run og` were run, so sitemap, feed, llms files, .md copies, JSON-LD (BlogPosting, FAQPage, Speakable), Fedra knowledge and share images already include them.
- **Article phase 2 (5 more guides).** `image-converter-formats` (Image Converter), `image-resize` (Image Resizer), `background-remove` (Background Remover), `ocr-image-text` (Image to Text OCR), `unit-conversion` (Unit Converter). Written from what each tool really does today (for example: Background Remover works by colour only, Image Resizer saves JPG, Image Converter handles one picture at a time). Five more original covers: `convertfmt`, `resizepx`, `cutout`, `ocrtext`, `unitscale`. `seo`, assistant index and `og` were re-run (132 articles, 5,822 knowledge passages).
- **Article phase 3 (5 more high-search guides).** `unlock-pdf` (Unlock PDF), `pdf-to-images` (PDF to Image Converter), `whatsapp-link` (WhatsApp Link Generator), `passport-photo` (Passport Size Photo Maker), `text-case` (Case Converter). Each is written around the questions people search (8 FAQs each, with a Hinglish question such as "pdf ka password kaise hataye"), from what the tool really does today (for example: Unlock PDF needs the real password and cannot break one; PDF to Image fills a white background and converts up to 60 pages at a time; Case Converter's Sentence case capitalizes only the first letter of the whole text). Five new original covers in `ArticleCover.jsx`: `unlockpdf`, `pdftoimg`, `chatlink`, `idphoto`, `casing`. Twelve older articles got one new "see also" paragraph linking to the new ones (merge-pdf, split-pdf, pdf-compress, jpg-to-pdf, image-resize, image-to-kb, background-remove, qr-generator, word-counter, ocr-image-text, heic-to-jpg, image-converter-formats). `seo`, assistant index and `og` re-run (137 articles, 5,902 knowledge passages).

## Top bar and one search for everything (v8)

- **Top bar** is now only the logo plus three quiet buttons: **Chains**, **History** and **Menu** (icon only on small phones, icon + name on wider screens; the page you are on is highlighted). The search button and the theme button are gone from the bar. On desktop the category row stays.
- **Menu** (`Header.jsx`): search box at the very top, then quick tiles (Chains, History, Saved files, Wishlist), the Light / Dark / Auto switch, then all categories. Typing in the search box replaces the list with results.
- **One search for the whole site** (`lib/globalSearch.js`, `GlobalSearch.jsx`): finds tools, your chains, saved files (Artifacts), what you entered or got in History, pages, and settings such as "dark mode". Typing only `chains`, `history` or `files` lists everything you have there. Chains, files and history are read from this browser the first time search is used, nothing is uploaded.
- **Home page search** is now a real box (it used to only open the header search) and shows the same grouped results right under it.
- `/` or `Ctrl+K` focuses the home search box, or opens the menu with the search box ready on other pages.

<!-- V9-START -->
## v9 — 78 more tools (same look, wired everywhere)

Every tool below uses the shared kits (`TextTool` / `CalcTool`), so it automatically gets: History capture, Artifacts saving, the Chain builder/runner (`tool-io.js`), Share link, Send to, the Wishlist star, global search (`globalSearch.js` reads `tools-lite.js`), category pages, the footer, `site-map`, `public/sitemap.xml`, `public/seo/*.html` and `llms.txt`.

For each tool the following were added: the route folder in `src/app/tools/<slug>/`, a `toolsRegistry` entry, a `guides.js` guide (how-to steps, tips, FAQ), an icon mapping in `ToolMark.jsx`. Derived files (`tool-components.js`, `tool-io.js`, `tools-lite.js`, sitemap, SEO pages) are rebuilt by `npm run build` (prebuild) and were also regenerated in this zip.

**Security Tools (7):** Atbash Cipher, Rail Fence Cipher, A1Z26 Cipher, ROT47 Encoder & Decoder, Affine Cipher, Polybius Square Cipher, Bacon Cipher

**Converter Tools (1):** Braille Translator

**Text Tools (13):** Mirror Text Generator, Vaporwave Text Generator, Superscript & Subscript Text Generator, Underline Text Generator, Duplicate Character Remover, Paragraph Counter, Longest Word Finder, Pangram Checker, Syllable Counter, Smart Quotes Converter, Extract Text Between Two Characters, Remove Numbers from Text, Text Splitter

**Utility Tools (1):** Scrabble Score Calculator

**Developer Tools (1):** Base58 Encoder & Decoder

**Calculator Tools (28):** Loan Prepayment Calculator, Credit Card Payoff Calculator, Debt Payoff Calculator, FIRE Calculator, Emergency Fund Calculator, Net Worth Calculator, Dividend Yield Calculator, Stock Profit & Loss Calculator, Rental Yield Calculator, Down Payment Calculator, Leave Encashment Calculator, Overtime Pay Calculator, Freelance Hourly Rate Calculator, Subscription Cost Calculator, Gold Jewellery Price Calculator, EV vs Petrol Car Running Cost Calculator, Car Depreciation Calculator, Sales Tax Calculator, TDS Calculator, Macro Calculator, Daily Protein Intake Calculator, Waist to Hip Ratio Calculator, Body Surface Area (BSA) Calculator, One Rep Max (1RM) Calculator, Race Time Predictor, Steps to Distance & Calories Calculator, Lean Body Mass Calculator, Baby Age Calculator

**Math Tools (27):** Percentile Calculator, Quartile & IQR Calculator, Linear Regression Calculator, Correlation Coefficient Calculator, Confidence Interval Calculator, Normal Distribution Calculator, Binomial Probability Calculator, Poisson Distribution Calculator, Geometric & Harmonic Mean Calculator, Digit Sum & Digital Root Calculator, Root Calculator, Exponent Calculator, Modulo Calculator, Divisibility Checker, Special Number Checker, Collatz Sequence Calculator, Pascal’s Triangle Generator, Prime Number Generator, Ordinal Number Converter, Complex Number Calculator, Vector Calculator, Linear Equation Solver, Regular Polygon Calculator, Arc Length & Sector Area Calculator, Proportion Calculator, Percent Error Calculator, Fraction to Decimal & Percent Converter

<!-- V9-END -->

## Quick Start

```bash
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

## Deploy (Cloudflare Pages)

The site is a static website hosted on Cloudflare Pages. In Cloudflare: Workers & Pages > Create > Pages > Connect to Git, pick this repository, then set
- Framework preset: None
- Build command: `npm run build`
- Build output directory: `out`
- Environment variable: `NODE_VERSION` = `22`

Security headers and redirects are in `public/_headers` and `public/_redirects`; there is no `functions/` folder, so nothing runs on Cloudflare's servers (no Workers requests, no cost). The country is a guess from the device time zone and language (`src/lib/region.js`).
When you buy a new domain, add it under the project's Custom domains and run `node scripts/change-domain.js https://your-new-domain.com`, then build and push.

## Tools Included

| Tool | Route | Status |
|------|-------|--------|
| Image Compressor | /tools/image-compressor | Working |
| PDF to Word | /tools/pdf-to-word | Working |
| Word Counter | /tools/word-counter | Working |
| QR Generator | /tools/qr-generator | Working |

## Latest update — 109 new tools (total 299+) + 7 new categories

**New categories:** SEO Tools (10), Security Tools (10), Converter Tools (15), Math Tools (14), Social Media Tools (10), Audio & Video Tools (8), Business Tools (5).
**Added to existing categories:** Image Tools (+11: cropper, rotate/flip, filters, watermark, meme, favicon, color extractor, social resizer, grid splitter, SVG→PNG, rounded corners), PDF Tools (+10: merge, split, extract, delete pages, rotate, page numbers, watermark, image→PDF, PDF→image, text→PDF), plus Text, Developer, Design, Calculator and AI extras.

New shared building blocks: `src/components/LabKit.jsx` (file/image/PDF/media tools, loads pdf-lib / pdf.js from cdnjs only when needed) and `src/components/DocLab.jsx` (invoice, receipt, resume, certificate — live preview + print/Save as PDF). `ToolKit` now supports async `run`, plus `range` and `textarea` fields.
All tools are registered in `tools-registry.js`, have guides in `guides.js`, icons in `ToolMark.jsx`, and appear automatically in header, footer, category pages, search, history, sitemap, llms.txt and `public/seo/*.html` (`npm run seo` runs before every build).

## Earlier batch — 62 new tools (total 191)

**Text (15):** Title Case Converter, Sentence Counter, Duplicate Word Finder, List to Comma Separated Converter, Text Wrapper, Leetspeak Converter, Zalgo Text Generator, Acronym Generator, Character Frequency Counter, Line Shuffler, Vowel & Consonant Counter, Readability Score Checker, Bionic Reading Converter, Word Sorter, Punctuation Remover
**Developer (10):** JSON Key Sorter, JSON to YAML, JSON to TypeScript, JSON Compare, Base32 Encoder/Decoder, Robots.txt Generator, Sitemap XML Generator, htaccess Redirect Generator, CSV to SQL INSERT
**Utility (5):** Luhn Number Validator, Email Validator, Random String Generator, Username Generator, Time Zone Converter
**Calculators (23):** PPF, Lumpsum, CAGR, Inflation, Gratuity, HRA Exemption, Savings Goal, Salary Hike, Ideal Weight, Body Fat, Heart Rate Zones, Running Pace, Percentage Change, Ratio, Quadratic Solver, Volume, Break-Even, Sales Commission, Time Calculator, Sleep Cycle, Permutation & Combination, Leap Year Checker, Working Days
**Design (7):** Random Color Generator, Color Shades & Tints, Color Mixer, Border Radius, Text Shadow, Glassmorphism, CSS clamp()
**AI (3):** AI API Cost Calculator, Prompt Template Filler, AI Text Chunker

Every new tool follows the same pattern as the older ones: registry entry, `page.jsx` + tool component built on `ToolKit` (`TextTool` / `CalcTool`), a visual guide + FAQ in `src/lib/guides.js`, an icon mapping in `ToolMark.jsx`, and automatic inclusion in the header/menu, category pages, footer, search, history, `public/seo/*.html`, `sitemap.xml`, `llms.txt` and the Site Map page (`npm run seo` runs before every build).

## Tools added in this update (41)

**Text:** Line Sorter, Duplicate Line Remover, Whitespace Cleaner, Text Reverser, Find and Replace, Line Number Adder, Text to Binary, Morse Code Translator, Word Frequency Counter, Line Break Remover, Fancy Text Generator, Email & URL Extractor, Number to Words, Roman Numeral Converter
**Developer:** UUID Generator, JWT Decoder, JSON to CSV, Markdown to HTML, String Escape/Unescape, HTTP Status Code Lookup, Cron Expression Explainer, HTML to Text, SQL Formatter, URL Parser
**Calculators (new category):** Percentage, Age, Date Difference, BMI, EMI, GST, SIP, Discount, Simple & Compound Interest
**Design:** CSS Gradient Generator, Box Shadow Generator, Color Contrast Checker, Color Palette Generator
**Image:** Image to Base64, Base64 to Image
**Utility:** Random Picker, Random Number Generator

Each new tool has: a registry entry, a `page.jsx` + tool component, a visual guide/FAQ in `src/lib/guides.js`, an icon in `ToolMark.jsx`, and is included automatically in `public/seo/*.html`, `sitemap.xml`, `llms.txt` and the Site Map page by `npm run seo` (also runs on every build).

Most new tools use the shared `src/components/ToolKit.jsx` (`TextTool` for text-in/text-out, `CalcTool` for calculators) so they keep the same layout as the older tools: input box → run button → loading → result with Copy.

## Site-wide features (latest)

| Feature | Where it lives | Notes |
|---|---|---|
| **Installable + offline (PWA)** | `public/sw.js`, `public/offline.html`, `components/PwaRegister.jsx`, `public/manifest.webmanifest` | Pages a visitor has opened keep working offline; cdnjs libraries (pdf-lib etc.) are cached too. Shows an "Install app" card after ~25 s. Bump `VERSION` in `sw.js` to force-refresh every visitor's cache. |
| **Dark mode** | `components/ThemeToggle.jsx`, `lib/theme.js`, bottom of `app/globals.css` | Header button + mobile menu row. Follows the device setting until the visitor picks one. Colour tools (`data-keep-light`) stay light so swatches are accurate. |
| **`/` and Ctrl+K search** | `components/Shortcuts.jsx` | Fires the existing `toolcloak:open-search` event handled by the header. |
| **Share result link** | `components/ShareLink.jsx`, used inside `ToolKit.jsx` (`TextTool`, `CalcTool`) | Inputs are packed into the URL `#s=…` (never sent to a server). Opening the link restores the values and runs the tool. Custom tools that don't use `ToolKit` don't get it yet. |
| **Drag & drop + paste** | `components/ToolBridge.jsx` (mounted in `ToolPageLayout`) | Drop a file anywhere on a tool page or press Ctrl+V to paste a screenshot. Works for every tool that has a file picker, with no per-tool code. |
| **Send result to another tool** | `components/SendTo.jsx`, `lib/pipe.js` | "Send to ▾" on text results and on image/PDF/audio/video downloads. Edit the suggestions in `TEXT_NEXT` / `FILE_NEXT` inside `lib/pipe.js`. |
| **History backup** | `lib/history.js` (`exportBackup`, `importBackup`), `components/HistoryClient.jsx` | Export/Import JSON of history, saved results and wishlist from the History page. Import merges — nothing is overwritten. |

## How to Add a New Tool

### Step 1 — Register in `src/lib/tools-registry.js`
```js
{
  slug: "your-tool-name",        // URL: /tools/your-tool-name
  name: "Your Tool Name",
  description: "Full description for SEO meta",
  shortDesc: "Short description",
  icon: "",
  category: "Category Name",
  color: "#hexcolor",
  emoji: "",
  keywords: ["keyword1", "keyword2"],
}
```

### Step 2 — Create folder and files
```
src/app/tools/your-tool-name/
  page.jsx          ← SEO metadata + ToolPageLayout wrapper
  YourToolTool.jsx  ← 'use client' — actual tool UI
```

### Step 3 — Done!
The sidebar will automatically show your new tool. No other changes needed.

## Project Structure

```
src/
├── app/
│   ├── layout.jsx              ← Root layout (Header + Sidebar)
│   ├── globals.css             ← Global styles
│   ├── page.jsx                ← Redirects to first tool
│   └── tools/
│       ├── image-compressor/   ← Tool 1
│       ├── pdf-to-word/        ← Tool 2
│       ├── word-counter/       ← Tool 3
│       └── qr-generator/       ← Tool 4
├── components/
│   ├── Header.jsx              ← Site header with logo
│   ├── Sidebar.jsx             ← Auto-generated from registry
│   └── ToolPageLayout.jsx      ← Reusable tool page layout
└── lib/
    └── tools-registry.js       ← Central tool registry
```

## AdSense Setup

1. Apply at https://adsense.google.com
2. Add your AdSense script to `src/app/layout.jsx` in the `<head>`
3. Place ad units inside `ToolPageLayout.jsx`

## SEO Features

- Individual metadata per tool page
- OpenGraph tags
- Keywords per tool
- Breadcrumb navigation
- FAQ sections (rich results eligible)
- Article content for each tool
- Internal linking between tools
- robots: index, follow

## Contact
The site has a feedback form at /feedback. Sending is switched off on purpose (nothing is sent to an outside service). No email address is written in the code.

## Performance & UX update
- Menus (desktop panel + mobile drawer) load tools in chunks as you scroll, with a search box inside every category.
- "More Free Tools" on tool pages builds a category list only when opened; guide + more-tools sections use `content-visibility:auto`.
- Home search box opens the header search (event `toolcloak:open-search`).
- Total shown everywhere as `299+` via `TOOL_COUNT_LABEL` in `src/lib/tools-registry.js` (actual count: 300).

## Wishlist + speed update (v4)
- **Wishlist:** star button on every tool card (home, category pages, header menus, search results, tool page, "More tools") → `/wishlist` page. Saved in localStorage (`toolcloak_wishlist`). Code: `src/lib/wishlist.js`, `src/components/StarButton.jsx`, `WishlistClient.jsx`, CSS at the end of `globals.css`.
- **Speed:** browser bundle now uses `src/lib/tools-lite.js` (auto-generated by `scripts/generate-seo-pages.js` on every build, short descriptions only) instead of the full registry; home page no longer ships the tool list twice; fonts load non-blocking (logo fonts reduced to the letters of the wordmark); Google Analytics starts after first interaction (or 6 s); drawer content mounts on first open; entrance animation removed from the tool header/box (faster LCP).
- **Home:** smaller section gaps, 6 rotating category chips (+ Wishlist/History), "Recently used" is a vertical list of the last 5.

## v5 — 15 big tools, Compressor category, similar tools
- **New category "Compressor Tools":** PDF Compressor, Compress Image to Exact KB, Audio Compressor (MP3), Video Compressor, ZIP File Creator, ZIP Extractor.
- **Other new big tools:** PDF Page Organizer, Sign PDF, Image to Text (OCR, Hindi + English…), Photo Collage Maker, Passport Photo Maker (print sheet), Video Trimmer, Audio Joiner, Video to MP3, Speech to Text.
- Shared helpers: `src/components/AvKit.js` (MP3 encode via lamejs, audio merge, video re-record). Libraries load from cdnjs only when a tool needs them (pdf-lib, pdf.js, lamejs, JSZip, tesseract.js).
- **Similar tools:** every tool page shows 6 related tools under the tool box (`RelatedTools.jsx`, server-rendered, picked by shared words + category).
- **Home:** wishlist tools appear right under the search/chips block.
- Logo fonts restored (Outfit / Righteous / Fredoka / Space Grotesk) — loaded non-blocking in the main font sheet.
- Total is now 315 tools in 16 categories; site text still says 299+ (`TOOL_COUNT_LABEL`).

## v6 — 34 more tools (total 349, site text still "299+")
Skipped as duplicates of existing tools: PDF Compressor, PDF Page Number Generator, Text Diff, Duplicate Line Remover, Text Frequency Analyzer, Reading Time Calculator, Slug Generator, Compound Interest, Break-even, ROI, SIP Goal, AI Prompt Optimizer, AI API Cost Estimator, Unix Timestamp, TOTP Generator, Video Compressor.
- **Image:** Image Blur, Image Pixelate, EXIF Viewer (+ clean copy), Color Blindness Simulator.
- **PDF:** PDF to Excel, PDF Metadata Remover, PDF Form Filler, PDF Compare.
- **Developer:** YAML Formatter / Validator / YAML→JSON, XML→JSON, JavaScript Formatter / Minifier, API Request Builder, Regex Builder, HTTP Headers Parser, DNS Lookup, CORS Tester, .gitignore Generator.
- **Design:** CSS Flexbox / Grid / Button / Animation generators (`CssLab.jsx`).
- **Calculators:** Loan Amortization Schedule, Retirement Corpus.
- **AI:** System Prompt Builder, Context Window Calculator, Prompt Injection Detector.
- **Others:** JWT Signature Verifier, SEO Audit Checker, YouTube Title Generator, Words to Number.
- Libraries loaded from cdnjs on demand: js-yaml, js-beautify, terser, SheetJS (xlsx). `@imgly/background-removal` loads from jsDelivr only in AI mode.

## v7 — big missing tools (batch 1: 14 new tools, total 409)
New shared helpers: `src/components/NewKit.js` (generic `FileTool`, result cards, chain/history-friendly), `SheetKit.js` (SheetJS + CSV parser), `CryptoKit.js` (AES-256-GCM file encryption), `AudioFx.js` (pitch-keeping time stretch, gain, fades). These files use `createElement` (`h`) instead of JSX so they can be syntax-checked with plain Node.
- **Developer:** Excel to JSON, CSV to Excel, JSON to Excel, Excel to CSV (SheetJS loads from cdnjs on demand).
- **Security:** File Hash Checker (SHA-256/384/512/1 + compare), File Encryptor & Decryptor (AES-256-GCM, PBKDF2).
- **Utility:** QR Code Reader (BarcodeDetector + jsQR), Barcode Generator (JsBarcode).
- **Image:** Signature Maker (draw/type → transparent PNG).
- **Audio & Video:** Audio Converter (MP3/WAV), Audio Speed Changer (keep pitch), Audio Volume Booster & Normalizer.
- **PDF:** PDF Multiple Pages per Sheet & Booklet Maker, Crop PDF.
Each tool is wired the same way as the older ones: registry entry, `page.jsx` + component, guide + FAQ in `guides.js`, icon in `ToolMark.jsx`, chain info in `scripts/generate-tool-io.js`, "Send to" suggestions in `pipe.js`, and automatic menu / category / search / history / sitemap / llms.txt / `public/seo/*.html` via the prebuild scripts. `sw.js` VERSION bumped to `tc-v3`; site label now says 400+.

## v8 — 32 more big tools (total 441, site text says "440+")
Added with the same wiring as every other tool (registry entry, `page.jsx` + component, guide + FAQ in `guides.js`, icon in `ToolMark.jsx`, chain info in `scripts/generate-tool-io.js`, and automatic menu / category / search / history / sitemap / llms.txt / `public/seo/*.html` through the prebuild scripts). `sw.js` VERSION is now `tc-v4`.
- **Utility:** Typing Speed Test, Spinner Wheel, Online Notepad, Countdown Timer, Pomodoro Timer, Click Speed Test, Reaction Time Test, Keyboard Tester, Webcam Test, Microphone Test, World Clock, WiFi QR Code Generator, UPI QR Code Generator.
- **Calculators:** In-Hand Salary (CTC → take-home), Stock Average, NPS, Paint.
- **Business (print / Save as PDF):** Rent Receipt (HRA), Salary Slip, Marriage Biodata.
- **Developer:** JSON Schema Generator, cURL → fetch/Axios/Python, SVG Optimizer, HTML → JSX.
- **Text / Image / Design:** Text Summarizer, Text to Handwriting, Image Joiner, Image to ASCII Art, Word Cloud Generator.
- **PDF / Audio & Video / Social:** Add Text to PDF, Video to GIF (uses `GifKit.js`).
Interactive tools (timers, tests, notepad, QR makers…) are marked "not chainable" through the `blocked` list in `generate-tool-io.js`.

## New in this version

- **Chain templates** (`src/lib/chain-template-data.js`, `chainTemplates.js`, `components/ChainTemplates.jsx`, `app/chains/templates/[slug]`): ready-made chains, each checked against the chain rules at load. A template whose steps cannot connect is dropped. Each has its own SEO page (title, description, HowTo / Breadcrumb / FAQ schema) and is in the sitemap.
- **Batch mode** (`components/ChainBatch.jsx`): several files, the chain runs on each one by itself, progress `3/20 done`, one failed file never stops the rest, per-file retry from the failed step, all results in one ZIP. One-file-at-a-time steps and joining steps (Merge PDF...) get a clear message.
- **Result preview / resume** (`lib/chainRuns.js`): every finished step shows a quick look and "Run again from here". Finished steps are also kept in IndexedDB (only if storage is accepted, same rule as History), so a failed or closed run can be resumed.
- **Share link + QR** (`lib/chainShare.js`, `lib/qr.js`): compact links (`#c=`), untouched templates as `#t=name`. QR is made in the browser (no third party). Opening a link adds the chain and opens it ready to run.
- **Before / after card** (`lib/runSummary.js`, `components/ChainSummary.jsx`): sizes, counts and time only, as a PNG with the logo.
- **Theme Auto** (`lib/theme.js`, `ThemeToggle`, `ThemeGuard`): Light / Dark / Auto, stored in `toolcloak_theme`.
- **Alerts** (`lib/notify.js`, `components/AlertSettings.jsx`): chime and optional browser notification, key `toolcloak_alerts_v1`.
- **Suggestions** (`lib/chainSuggest.js`): "Often used after ..." in the chain editor, only steps that pass `canFollow`.
- **Tap to enlarge** (`components/Zoom.jsx`, `lib/zoomEvent.js`): file previews, result pictures and the Send-to result tile open the big viewer.

## v11 — site housekeeping (Oct 7, 2026)

- **Info pages refreshed** (`src/lib/info-pages.js`, date now October 7, 2026): About lists all 16 real categories with counts and no longer mentions a Science category (it has no tools yet); Privacy and Cookie Policy list every item kept on the device (history, results/files, chains, wishlist, pins, theme, alerts, notepad, offline cache) and every outside service (Google Analytics, Fonts, qrserver for the three QR tools, cdnjs/jsDelivr helper libraries, DNS/YouTube/API tools); Disclaimer, FAQ and How it works match.
- **Counts**: `TOOL_COUNT_LABEL` is now `540+` (actual: 547); title/description in `layout.jsx` and `seo.js` match; `llms.txt` uses the label instead of a fixed number.
- **Sitemap**: regenerated (586 URLs, all dated today). The generator now skips categories that have no tools (Science Tools), also in `/site-map`, the category list and the home JSON-LD. `public/sitemap.xml` is rebuilt on every `npm run build`.
- **Menu**: opens at about half the screen on phones and tablets (min 280px, max 420px); the corner button next to the close button makes it fill the whole page, press again to shrink. It always opens at half size.
- **Cookie banner** (`CookieConsent.jsx`): compact bottom card, short Accept / Decline wording, "What else is stored?" fold-out, safe-area aware, side-by-side layout for phones held sideways. Behaviour (what Accept and Decline do) is unchanged.

## v12 — Science Tools (9 new tools, total 556, label "550+")

New category now has tools: Ohm's Law, Resistor Color Code (4-band), Projectile Motion, Ideal Gas Law (PV=nRT), Molarity, pH, Molar Mass (formula parser with brackets and hydrates), Half-Life, Force (F=ma). All are `CalcTool` tools (accent #0891b2), wired like every other tool: registry entry, `page.jsx` + component, guide in `guides.js`, icon in `ToolMark.jsx` (`BY_NAME`), and picked up automatically by search, chains (`tool-io.js`, `tool-components.js`), history, category page, sitemap, `public/seo`, `llms.txt` and the info pages (About lists Science Tools (9)).
- **Send to box**: every tool in the list (suggested and search results) now shows its own logo tile (`SendTo.jsx`).
- **Chains page**: added the Home › Chains breadcrumb at the top (`ChainsHome.jsx`), like the other pages.

## v19: Blog is now Articles
- The old "blog" is renamed to **Articles** everywhere: route `/articles` and `/articles/<slug>`, `src/content/articles/`, `src/lib/articles*.js`, `Article*` components, `public/articles/*.md`, `public/seo/articles/`, menu, footer, home strip, search, sitemap, RSS and llms files. `netlify.toml` 301-redirects the old `/blog` and `/blog/*` to the new addresses. CSS classes keep their short `bl-` prefix on purpose (internal only). Schema.org types `Blog` and `BlogPosting` are kept, because those are fixed names that search engines expect.
- **Nine new articles** (rule of 72, percentage change, credit card interest, BMI, CAGR, aspect ratios, hashes, Unix time, inflation), each tied to one tool, each with an "In short" box, graphs and drawings.
- **New figure types** for articles (`{ t: 'fig', kind: ... }`): `donut`, `cols` (vertical columns), `stats` (big-number cards), `scale` (coloured ruler with a marker), `compare` (two sides), `timeline`, `frames` (aspect ratios drawn to scale). Existing `bars`, `line`, `flow` still work. New block `{ t: 'tldr', items: [...] }` gives the "In short" box. All are pure SVG/CSS with light and dark versions (`ArticleVisuals.jsx`, `articleCss.js`).
- Eight new cover drawings: doubling, percent, scale, card, hash, frames, clock, shrink (`ArticleCover.jsx`).
- The HTML sitemap page (`/site-map` and `public/seo/site-map.html`) now lists every article.
- To add an article: copy a file in `src/content/articles/`, import it in `src/lib/articles.js`, run `npm run seo`.

## v20: eight more articles, richer visuals
- **Eight new articles**: step-up SIP, loan prepayment (one extra EMI a year), password entropy, the 25x FIRE rule, stacked discounts, Flesch reading ease, sleep cycles, UUID collision odds. Each is tied to one tool, has an "In short" box, FAQs and several drawings. Files are in `src/content/articles/`, registered at the top of `POSTS` in `src/lib/articles.js`. Home strip, menu, search, `/site-map`, sitemap.xml, RSS and llms files pick them up automatically (`npm run seo`).
- **Four new figure types** (`{ t: 'fig', kind: ... }`, all pure SVG/CSS, light and dark): `gauge` (half-circle meter: `min`, `max`, `value`, `big`, `zones[{label,from,to,color,shown}]`), `dots` (x out of 100: `total`, `per`, `groups[{label,n,color,shown}]`), `funnel` (`items[{label,v,shown,color}]`), `quad` (2x2 boxes: `items[{t,d,color}]`, `xlabel`, `ylabel`). Code: `ArticleVisuals.jsx`, styles `V20` in `articleCss.js`, text fallback for the SEO pages in `scripts/generate-seo-pages.js`.
- **Eight new cover drawings**: stepup, prepay, entropy, fire, discount, readable, moon, uuid (`ArticleCover.jsx`).
- **Second v20 batch, eight more articles**: PPF, FD after tax and inflation, percent vs percentage points, HTTP status codes, px vs rem, cron expressions, JWT, gratuity (15/26). They reuse existing covers. 39 articles in total.

## v22: richer articles, four new figure types
- **New articles** (long form, each tied to one tool, with "In short" box, FAQs and 5 to 8 drawings): compounding (start at 25 vs 35), markup vs margin, HRA exemption, break-even point, heart rate zones, CIDR and subnets, CTC to in-hand salary, SWP (how long a corpus lasts), binary/hex/decimal, appliance electricity cost (14 new, 53 in total), plus lumpsum, savings goal, running pace and Caesar cipher. Files are in `src/content/articles/`, registered at the top of `POSTS` in `src/lib/articles.js`. Home strip, menu, search, `/site-map`, `sitemap.xml`, RSS, `llms*.txt` and the per-article SEO pages are rebuilt by `npm run seo` (already run in this zip).
- **Four new figure types** (`{ t: 'fig', kind: ... }`, pure SVG/CSS, light and dark, code in `ArticleVisuals2.jsx`, styles `V22` in `articleCss.js`, text fallback in `scripts/generate-seo-pages.js`):
  - `area`: filled area chart. `x`, `series[{name,y,color}]`, optional `stack: true`, `ydiv`, `yunit`, `every`, `marks[{at,label}]`.
  - `stacked`: stacked bars. `series[]` (legend), `items[{label,total,parts[{v,shown,color}]}]`, `abs: true` for one shared scale.
  - `waterfall`: `items[{label,kind:'total'|'minus'|'plus',v,shown}]`.
  - `heat`: shaded table. `corner`, `cols[]`, `rlabels[]` (row names), `values[][]`, `cells[][]` (text), `hot:[row,col]`, `legend`. Note: row names are `rlabels` (not `rows`) so the text/SEO helpers do not mistake them for table rows.
- **Six new cover drawings**: tag, pulse, network, house, seesaw, tree (plus wallet), in `ArticleCover.jsx`.

## v23: nine longer articles, six new cover drawings
- **New articles** (long-form, tied to one tool, with "In short" box, FAQs and 9 to 14 drawings): storage units (why 1 TB shows 931 GB, KB vs KiB, MB vs Mbps), the bell curve (68-95-99.7 rule and z-scores), mean vs median vs mode, recurring deposit maturity, prime numbers (and RSA), AI tokens and API cost, the Fibonacci sequence, Celsius/Fahrenheit/Kelvin, and how TOTP authenticator codes work. 62 articles in total. Files are in `src/content/articles/`, imported at the top of `POSTS` in `src/lib/articles.js` under `// v23`.
- **Six new cover drawings** in `ArticleCover.jsx`: `bell`, `drive`, `coins`, `chip`, `primes`, `spread`.
- **Wired everywhere automatically** (nothing extra to edit): the tool page "Full guide" card and guide tiles, the home strip, the menu list, global search (`lib/articlesIndex.js` indexes every section), the articles page and its search box, `/site-map`, `sitemap.xml`, `llms.txt`, `llms-full.txt`, `feed.xml`, `public/articles/*.md` and `public/seo/articles/*.html`. `npm run seo` was already run in this zip and runs again before every build.
- The articles use only the existing figure types (`bars`, `line`, `area`, `stacked`, `donut`, `cols`, `stats`, `scale`, `waterfall`, `heat`, `funnel`, `quad`, `dots`, `flow`, `compare`, `timeline`) so no new chart code was needed.

## v24: ten more articles, ten new covers, toolcloak watermark
- **Ten new articles** (each tied to one tool, with an "In short" box, 7 to 10 drawings or tables and 6 FAQs): Morse code, Roman numerals, two dice and a coin, permutations vs combinations, GCD and LCM, BMR and TDEE, averaging down (stock average), ROI, half-life, and HEX/RGB/HSL colours. 72 articles in total. Files are in `src/content/articles/`, imported under `// v24` in `src/lib/articles.js`. The tool page "Full guide" card, guide tiles, home strip, menu, global search, articles page and its search box, `/site-map`, `sitemap.xml`, `llms.txt`, `llms-full.txt`, `feed.xml`, `public/articles/*.md` and `public/seo/articles/*.html` pick them up automatically. `npm run seo` was already run in this zip (and runs before every build).
- **Ten new cover drawings** in `ArticleCover.jsx`: `morse`, `roman`, `dice`, `combo`, `gears`, `flame`, `candles`, `target`, `decay`, `palette`.
- **Watermark**: every cover (new and old) now carries a small "toolcloak" mark at the bottom right (`<g className="bl-wm">` at the end of `ArticleCover`). It is part of the drawing, so it shows on article pages, cards, tiles and anywhere the cover is used. Edit or remove it in that one place.
- Figures used: `bars`, `cols`, `line`, `area`, `waterfall`, `stats`, `flow`, `compare`, `quad`, `timeline`, `heat`, `dots`, `scale`, `gauge`, `donut`, `funnel` plus tables and callouts. No new chart code was needed. Note: multi-series `bars` stack their values, so compare two numbers as separate rows.
- `public/sw.js` VERSION is now `tc-v6`.


## Region, currency and translate

- **Country**: guessed on the device from the time zone, then the language region (no server, no outside lookup). The visitor can choose a country in Settings.
- **Currency**: all content is written in rupees. `src/components/RegionEngine.jsx` rewrites rupee amounts in page text (₹, Rs, lakh, crore, chart labels) into the visitor's currency with live rates (`open.er-api.com`, backup `jsdelivr currency-api`, cached 12 h). Original text is remembered, so switching back restores it. Tools keep their own handling in `ToolKit.jsx` (`CalcTool`: symbol, converted starting amounts, results in the visitor's currency).
- **Stay in rupees**: pages that follow Indian law are listed in `src/lib/regionData.js` (`INDIA_TOOLS`) and never convert.
- **Where to change it**: Menu > Settings > Region & currency / Translate website, Settings page > "Region, currency and language", and the two buttons at the top of every article.
- **Translate**: Google's page-translate widget, driven by `src/lib/translate.js`. Site-wide from Settings or the menu; article-only from the article's button (menu, header and footer are marked `translate="no"`, and the translation is dropped when you leave the article).

## v38: new articles batch (visual-first)
- **New articles**: Pythagorean theorem (3-4-5, ladders, TV sizes), projectile motion (why 45 degrees), dividend yield (yield trap, payout ratio, reinvesting), sample size (margin of error, the 385 rule). Each is tied to one tool, with an "In short" box, 10 to 14 drawings/tables and 6 FAQs. Files in `src/content/articles/`, imported under `// v38` in `src/lib/articles.js`.
- **Four new cover drawings** in `ArticleCover.jsx`: `triangle`, `arc`, `crowd`, `pascal`.
- Menu, home strip, tool page guide card, search, `/site-map`, `sitemap.xml`, `llms*.txt`, `feed.xml` and the per-article SEO pages all read the same registry; `npm run seo` regenerates them (already run in the full zip, and again before every build).
- **Second v38 batch**: linear regression (best-fit line, R squared, residuals), Pascal's triangle (patterns, coin flips, binomial expansion), waist-to-hip ratio, and sitemap.xml (limits, what to include, HTML vs XML sitemap). 8 new articles in this version, 86 in total.


## Articles v41

- 12 new posts (98 total): IBAN validator, percent error, hreflang (supplied), plus loan amortization, z-score, chmod, robots.txt, Ohm's law, confidence interval, quadratic equation, CGPA to percentage and UTM links. Each is one file in `src/content/articles/`, imported in `src/lib/articles.js` (newest first). `npm run seo` (also before every build) adds them to the sitemap, `llms.txt`, `llms-full.txt`, `feed.xml`, `public/seo/articles/*.html` and `public/articles/*.md`. Site search, the articles page, tool-page guide cards and related tiles pick them up automatically. Service worker bumped to `tc-v8`.

## v42 (batch 1)

- **Currency fix for articles.** Chart axes and values written in thousands (`k`) are now converted with the visitor's currency, the same way lakh (`L`) values already were: `convertLakhAxis` in `lib/region.js` handles `L` and `k`, line/area charts mark both with `data-fxl`, and titles such as "(₹ thousand)" become the currency name. `loan-amortization` and `emergency-fund` got `prefix: "₹"` on their `k` column charts. When writing an article, put `₹` in front of every amount (or use `unit`/`yunit` of ` L` or `k` with a ₹ prefix) so it converts; guides for Indian-law tools stay in ₹ (`INDIA_TOOLS`).
- **Plain pages.** Chains, History, Wishlist and Saved files no longer have the big dark/orange banner or the tinted page background. `PageKit.jsx` (`Hero`, `.pk`) and `ChainsHome.jsx` (`.ch`, `.ch-hero`) now show a simple header on the normal page background.
- **Age Calculator fix.** A birth day of 31 (or 30, 29) compared with a date in a shorter month no longer gives negative days; whole months are counted first, then the days left.
- **6 new articles:** tip & bill split, VAT (add/remove), salary hike, net worth, subscription cost, age. Each is registered in `lib/articles.js` and picked up by search, tool pages, related guides, sitemap, `llms.txt`, `llms-full.txt`, the RSS feed and `public/seo/articles/`. Run `npm run seo` (also runs before every build) after adding more.

## v42 article batch 1 (15 more guides)

Fifteen new posts in `src/content/articles/`, each tied to one calculator and wired through `src/lib/articles.js` (newest first), so they appear on `/articles`, the home page cards, article search, the menu, related guide tiles, `sitemap.xml`, `llms.txt`, `llms-full.txt`, `feed.xml`, `public/seo/articles/` and `public/articles/*.md` (all rebuilt by `npm run seo`).

Tools covered: rental-yield-calculator, freelance-rate-calculator, ev-vs-petrol-savings-calculator, overtime-pay-calculator, down-payment-calculator, gpa-calculator, unit-price-calculator, days-until-date-calculator, working-days-calculator, profit-margin-calculator, speed-distance-time-calculator, final-grade-calculator, commission-calculator, pro-rata-calculator, weighted-average-calculator.

## Speed pass (performance)

- `LayoutShell.jsx`: only Header, Footer, ThemeGuard and SettingsApply are in the main bundle. Cookie banner, analytics, PWA, region, toast, zoom, swipe, shortcuts, progress bar etc. are separate chunks loaded after the page is idle (or on first touch/scroll/key). Dark colour engine loads at once only when the dark theme is on.
- `Header.jsx`: menu (`MenuDrawer`) loads on first open / hover; the desktop search box is `HeaderSearch.jsx`, loaded on wide screens only after idle. This removed the tool list + article covers from every page's first load.
- Fonts: 3 families only (Sora, DM Sans, DM Mono). The logo (`Wordmark.jsx`) is plain two-tone Sora text, `LogoMark.jsx` has flat colours (no gradients). Articles no longer download Newsreader (system serif is used) and no render-blocking font link.
- Articles: smaller title / intro / body sizes, poster is smaller and sits under the first paragraph, big blocks use `content-visibility:auto`. Fixed: `.bl-body h2` style only applied in dark mode.
- Home: smaller hero, sections below the first screen use `content-visibility:auto`, the articles strip uses a lighter CSS bundle (`homeCss`).
- `globals.css`: tool box glow is static (no endless spinning / pulsing animation); lighter shadows.
- `RegionEngine.jsx`: no page walk and no always-on MutationObserver while the currency is rupees.
- `next.config.js`: `removeConsole` in production. `public/_headers`: long cache for `/_next/static/*`. `sw.js` VERSION bumped to tc-v9.

## Trail bar and swipe (v47)

- `TrailBar.jsx` + `lib/trail.js`: one breadcrumb bar for the whole site. It follows the way the visitor really went (Articles > Post, or Home > Tools > Category > Tool > Guide ...). Not shown on the five main pages (Home, Tools, Articles, Chains, Library; the numbered article list pages count as Articles). A small back arrow on the left goes one step back; tapping an earlier name jumps to it. Landing straight on a deep page starts the trail at its main page. The trail lives in sessionStorage. Each step is named from the page's big heading (`main h1`).
- The old per-page "Home > ..." lines stay in the HTML for search engines, but `main nav[aria-label="Breadcrumb"]` is hidden in `globals.css`. Any new breadcrumb nav must carry `aria-label="Breadcrumb"` to be hidden the same way.
- `lib/tabs.js`: `tabIndexOf` now returns an index only for the five main pages themselves (it used to match everything under them), so swipe no longer works inside categories, tools, articles, chains or History.
- `SwipeNav.jsx`: no rocket for swipes; the next page is requested at once while the old one slides away; if it is not there after ~140 ms, a skeleton of that page is shown; the new page then slides in. Rocket still shows when a link is tapped.


## Tool assistant, Phase 1 (v48)

A chat assistant (floating "Assistant" button on every page, and the `/assistant` page). The person types a task in plain English and may drop files. It works out the steps, shows the plan in plain words, asks before risky steps, runs the steps in the background and hands over the result. **Every fact or number comes from a tool, a calculator function (`calc.mjs`) or the site's own text. No language model, no server, no paid API, nothing is sent anywhere.**

- **Lazy.** `LayoutShell.jsx` loads only `assistant/AssistantLauncher.jsx` (one button) after the page is ready. The chat (`AssistantPanel.jsx`), the engine and the data load when the button is tapped; the knowledge file only when a how-to question is asked. Tools load through `toolComponents` when their step starts and are released afterwards.
- **Engine** (`src/lib/assistant/*.mjs`, plain JS, no React, tested in Node): `text.mjs` (stemmer, typo tolerance), `dictionary-en.mjs` (ALL English words and messages: add `dictionary-xx.mjs` for another language), `synonyms.mjs` (hand-written extra phrases for popular tools: only an addition), `search.mjs` (finds tools), `tasks.mjs` (splits "then / after that / commas / numbered lists"), `params.mjs` (quoted text, KB/MB, page ranges, formats, sizes, angles, mapped onto a tool's own options), `planner.mjs` (plan + validation with the real `chainRules.js`: `checkChain`, `suggestBridge`, `riskNotes`, `joinsFiles`, `worksOnOneFile`), `calc.mjs` (safe arithmetic parser, no eval; percentages, units, EMI, simple/compound interest, GST, discount, BMI ...), `answer.mjs` (how-to answers from articles and guides), `index.mjs` (`createAssistant({ index, rules, loadKnowledge }).handle(text, ctx)`).
- **Covers every tool.** `scripts/generate-assistant-index.js` runs in `prebuild` (after `generate-tool-io.js`) and writes `public/assistant/index.json` (every tool: name, descriptions, category, in/out kinds, stemmed search terms, readable options), `knowledge.json` (every tool guide step/tip/FAQ, every tool description and every article section, split into passages), `report.json` and `ASSISTANT-REPORT.md`. A new tool, guide or article is picked up on the next build with no manual work.
- **Runnable vs open-only.** Tools in `chainBlocked` (and any tool without a component or in/out info) are found and opened with an explanation ("a live tool, use it yourself"). All others run inside a chain.
- **How-to answers.** "How do I ...", "what is ...", "why ..." questions are searched in the guides and articles (BM25 with typo tolerance and synonyms), the best sentences are picked, de-duplicated and joined, and the sources are linked. If the passages do not really cover the question, it says it can only work with the tools on this site and suggests the closest tools.
- **Running.** The plan is saved as a normal chain (`saveChain`) and run by the existing `ChainRunner`, which now accepts two optional props: `chainId` and `assist`. With `assist` it shows no page, only its hidden tool box, starts by itself once the input (handed over with `setChainPipe`) is in, and reports `step-start`, `step-done`, `step-error`, `finish` and `stopped` through `assist.onEvent`; `assist.api` gives `retry`, `skip`, `stop`, `useManual`. Without `assist` nothing changed. The chat narrates the events, and a failed step offers Retry, Skip, or Do this step myself (`ManualSheet.jsx`). Several files with one-file-at-a-time steps run once per file; a joining step (`joinsFiles`, e.g. Merge PDF) runs after the earlier steps have run on each file. The result card has Download, Send to, Save to Artifacts and a one-line summary.
- `chainDom.restore` also understands settings with `by` (the start of a control's label) instead of a position, so the assistant can set "Target size" without knowing the control's index.
- **Gaps are visible.** `ASSISTANT-REPORT.md` lists per tool: runnable, open-only, or runnable but its options are not readable yet ("gap": it runs with its default options). To fix a gap, add the tool's options to `hand-params.mjs` (shape in the file) or write its options as a `FIELDS` list.
- **Tests:** `npm test` (`node --test tests/assistant.test.mjs`) runs the acceptance tests against the real index and the real chain rules (compress + watermark YES + merge, typo "mrge", "make photo under 50kb", "5 divide by 5", "20 percent of 850", Pomodoro opens, out-of-scope reply, bridge converters, coverage of every slug, every tool findable by its own name, no network or analytics calls in the engine).
- **Privacy.** The engine files contain no `fetch`, beacon or analytics call (a test checks it). The panel only loads static files from this site (`/assistant/*.json`). No Google Analytics event carries user text. Uploaded files stay in memory; only results can be saved, through the existing consent logic (`ensureConsent`, `putFile`, `addEntry`).
- **Known limits:** English only; rule based, so unusual phrasing can miss (it then asks a question with options or suggests tools); options are mapped only for tools whose options are readable (about 80 of 556 plus the hand-written ones, see the report); the plan can only join files at the start or as a joining step, not insert a new file in the middle; a page-bound tool that needs a click on something other than its main button can fail in the background (the error offers Do this step myself); the knowledge file is about 3 MB (1 MB gzipped), loaded only for how-to questions; the engine was tested in Node, so run the chat once in a browser after each deploy and check the Network tab.
- **Phase 2 (not built):** an opt-in "Turn on smart mode" button with a download-size notice and WebGPU / memory check, a small in-browser model (WebLLM or transformers.js, weights cached by the browser, removable) whose only job is to turn messy English into the same Plan JSON, validated against the valid slugs and settings, falling back to the rule engine on anything invalid. It would never produce facts or numbers.


## v54 notes: Fedra maths + home page speed

- **Maths in Fedra** (`src/lib/assistant/math.mjs`, card in `src/components/assistant/MathCard.jsx`): equations (linear, quadratic, higher, systems), derivatives, integrals, limits, statistics, gcd/lcm, primes, factorials, nCr, determinants, plots. Exact steps, SVG graph, and a button to the matching tool. No model, no eval. Tests: `npm test`.
- **Home page speed**: the home page no longer downloads the 146 KB full tool list. It reads `src/lib/home-lite.js` (about 11 KB) and `src/lib/categories-lite.js`; both are generated by `scripts/generate-seo-pages.js` on every build (the home page scans its own slugs, so editing the lists in `HomeClient.jsx` just works). The full list loads only for visitors who already have History or a Wishlist. The ready-made chains row (`HomeTemplates.jsx`) loads when it scrolls near. On the home page the swipe, shortcut, zoom, region and progress helpers wait for the first touch/scroll/key press (or 8 s).
- Fedra files `/assistant/*.json` are cached for an hour (public/_headers) and warmed when the Fedra button is touched.

## v55 — size fix, Fedra understanding, memory, tokens

- **Watermark no longer grows the file.** Image tools used to save every result as PNG (a JPG photo became ~4x bigger). `ImageLab` now keeps the source type (JPG stays JPG, WEBP stays WEBP) and steps the quality down until the result is no bigger than the original (`canvasBlobFit` in `LabKit.jsx`). PDF tools save through `guardPdfLib` (keeps the smaller of two save modes). New **Video Watermark Adder** re-records at the original's bitrate (`recordVideo(..., { bitrate: 'match', overlay })`). One shared painter: `drawWatermark`.
- **Tool options**: the assistant index now reads quoted `"fields": [...]` too, so 237 tools (was 73) have named options; 557 tools total.
- **Fedra understands first** (`asks.mjs`, `planner.mjs`, `hinglish.mjs`): Hinglish verbs and "phir / isme / uske baad" become steps; the plan card shows files, what each step will really do ("Make the file 70% smaller", "Watermark text: Yash"), and asks the missing value with tap-answers plus a box for your own (watermark text, format, size, pages, angle, password). Compression amounts show as optional tweaks.
- **Files**: a text file is read (its words are the data, or the instructions if you say "do what is written" or send it alone). A file with no instruction, or words that name no job, gets a "what should I do?" card with options. A job waiting for its file starts as soon as the file arrives.
- **Memory**: last watermark text and last job are remembered ("same again", "same watermark as before"), rebuilt when an old chat is opened.
- **Tokens**: every answer has a token (`FD-XXXXXX`) that seeds how it is composed (direct, whole-article summary, FAQ, steps, mixed). Asking again or pressing Regenerate gives a different version of the same site facts. "base 64" = "base64".
- **UI**: copy on every prompt and answer, copy plan, live step pipeline while running, per-step before/after sizes, dark article-points block.


## Privacy changes (v14)
- No outside CDN: libraries are copied from npm into `public/vendor/` by `scripts/copy-vendor.js` before each build.
- QR codes are drawn locally (`lib/qr.js`). No IP-lookup or exchange-rate service is contacted; currency uses built-in approximate rates.
- Feedback form sending, YouTube Thumbnail Downloader, the cloud AI background model and the STUN helper were removed.

## Embed system + Donate (v19)

- **Embed.** Every tool page has an "Embed this tool on your website" card a short scroll below the tool (`EmbedCard.jsx`): pick a height, copy the code. `/embed` lists every tool with its own Copy / Show code / Preview buttons, search and category filter (`EmbedHub.jsx`). The framed page is `/embed/<slug>` (`app/embed/[slug]`, `EmbedFrame.jsx`): only the tool plus a plain "Powered by ToolCloak" line linking back. No header, footer, cookie banner or analytics inside the frame (`LayoutShell.jsx`). `public/embed.js` is an optional one-line script that makes the frame fit the tool's height. Embedded pages are `noindex` with a canonical to the real tool page. No server: it is all static files.
- **Headers (Cloudflare Pages).** `public/_headers` gives `/embed/*` its own rule (removes `X-Frame-Options`, uses `frame-ancestors *`). The rest of the site stays frame-protected.
- **Donate.** A small SVG "Donate us" button sits under every tool (`DonateButton.jsx`; `DonateGate.jsx` decides when: tools with a Generate/Convert button or a file picker show it only after a result, tools that answer by themselves show it at once). Click -> choose country (India only for now) -> UPI ID with Copy, QR and a "Pay with UPI app" link. `/donate` is the dedicated page with amount chips, a bigger QR and Download QR (`DonateClient.jsx`). The QR is drawn in the browser (`lib/qr.js`).
- **UPI ID is set in ONE place:** `src/lib/donate.js` -> `upiId`. Also change `payeeName`, `note`, `amounts` there if you like. To add another country, add it to `DONATE_COUNTRIES` and a branch in `DonateButton.jsx`.
- Footer, search and sitemap list `/embed` and `/donate`.

## Sponsor slots, supporters, templates, custom tools (v20)

All of it is static (no server). The settings live in three small files, each with its "how to" written at the top.

- **Contact email: ONE place.** `src/lib/business.js` -> `BUSINESS.email`. Every "Contact to advertise / I want this / Email this request" button is a `mailto:` built from it. While it is empty, the buttons go to `/sponsor#contact`, which says the email is coming soon (no dead links). Also in that file: `NEWSLETTER.url` and the optional `PRICES` lines.
- **Sponsor slots** (`components/SponsorSlot.jsx`, `lib/sponsors.js`). Six places: `home`, `tool` (every tool page), `article`, `category`, `supporters`, `templates`. Empty = a dashed "Your ad can be here" box with a "Contact to advertise" button and a link to `/sponsor` (sizes and details). To sell one: put the picture or short .mp4 in `public/sponsors/`, add one entry to `SPONSORS` (image, video or text), push. `until: 'YYYY-MM-DD'` ends it by itself (checked in the visitor's browser, no rebuild). Safety rules enforced in code and tested: picture/video must be a file on this site (so no advertiser server ever sees a visitor), links must be `https`, every sponsor is labelled "Sponsored", links use `rel="sponsored nofollow noopener noreferrer"`. Recommended picture: 1200 x 300 (4 : 1). Not shown inside `/embed/*` frames.
- **Supporters** (`lib/supporters.js`, `components/SupportersStrip.jsx`, `/supporters`). The list is ONE text block: add a line `Name, Amount` on GitHub and commit; the site rebuilds. The same name twice is added together. Badges: Supporter / Champion (Rs 100+) / Hero (Rs 500+), editable in `TIERS`. Shows on the home page (hidden until the first name), on `/donate` and on `/supporters`. Every place says the list is typed by hand and not connected to UPI.
- **Templates shop** (`/templates`, `lib/templates-store.js`). Five packs, all "Coming soon" with an "I want this" email button. To sell one: set `status: 'live'`, `price`, and `buyUrl` (a Razorpay / Gumroad / Instamojo link). A product counts as live only with a price and an https link.
- **Custom tool page** (`/custom-tool`, `components/CustomToolForm.jsx`, `lib/customTool.js`). A request form that opens the visitor's mail app with the text filled in, plus a Copy button. Nothing is sent from the page.
- **Newsletter** (`components/NewsletterCard.jsx`). Hidden until `NEWSLETTER.url` is set; then a card appears on the home page and a link in the footer. It is a plain link to the newsletter service.
- Footer has a new "Support" block (Donate, Supporters, Advertise, Templates, Custom Tool); search and sitemap list the four new pages. The Privacy Policy now describes sponsor banners.
- Tests: `tests/business.test.mjs` (27).

## Articles batch 3 (15 search-led guides)

`nps-pension`, `height-cm-to-feet`, `upi-qr-code`, `ats-resume`, `gold-jewellery-price`, `simple-vs-compound-interest`, `signature-png`, `text-to-handwriting`, `steps-to-km`, `marriage-biodata`, `invoice-format-india`, `youtube-earnings`, `video-compress`, `ai-tokens`, `lakh-crore-format` (all in `src/content/articles/`, registered at the top of `POSTS` in `src/lib/articles.js`, 165 articles in total).

- Each is written around one high-volume question (English and Hinglish), with 8 FAQs, tables, figures, and links to related tools and older articles. 49 older articles got a "See also" paragraph that links back to the new ones.
- 15 new original covers in `ArticleCover.jsx`: `pension`, `heightscale`, `upiqr`, `resumecv`, `goldring`, `simplecompound`, `signpen`, `handwrite`, `footsteps`, `biodatacard`, `invoicebill`, `ytearn`, `videosqueeze`, `tokenchunks`, `lakhcrore`.
- `ArticleBlocks.jsx`: step text in `flow` figures now supports `[links](/..)` and bold (it showed raw markdown before).
- Re-run after the change: `generate-seo-pages.js` (sitemap, RSS, llms files, `.md`, JSON-LD), `generate-assistant-index.js` (Fedra: 6,333 passages), `npm run og` (share images). All article links were checked against the tool registry and the article list; assistant tests pass (31).
- Facts to re-check over time: NPS exit rules (Dec 2025 changes), GST rate on jewellery, YouTube Partner Program thresholds, token window sizes in the token counter.
- Known gap: the NPS tool's annuity field still starts at 40 percent (the article explains the 20 percent case by hand for non-government subscribers).

## Articles batch 4 (12 guides)

`color-converter-formats` (HEX/RGB/HSL, same tool as the older `color-formats`), `extract-pdf-pages`, `add-page-numbers-to-pdf`, `protect-pdf`, `delete-pdf-pages`, `rotate-pdf`, `json-to-csv`, `video-to-mp3`, `svg-to-png`, `click-speed-test`, `reaction-time-test`, `salary-slip-format` (all in `src/content/articles/`, registered at the top of `POSTS` in `src/lib/articles.js` under `// v43 batch 4`, 177 articles in total). 12 new cover drawings are in `ArticleCover.jsx` (batch 4). OG images are in `public/og/articles/`. Related-guide links were added to 20 older articles. `npm run seo` was run.

## Articles batch 5b (4 more guides)
- `remove-duplicate-lines` (Duplicate Line Remover), `remove-line-breaks` (Line Break Remover), `date-difference-calculator` (Date Difference Calculator), `random-name-picker` (Random Name Picker). Each is tied to its tool and has an "In short" box, 8 FAQs (one Hinglish), tables and figures. Four new original covers in `ArticleCover.jsx`: `dedupelines`, `linejoin`, `dategap`, `namedraw`.
- Registered at the top of `POSTS` in `src/lib/articles.js` under `// batch 5b`. Eight older articles got one "See also" paragraph linking to them (days-until-date, working-days, age-calculator, random-number-generator, dice-and-coins, word-counter, pdf-to-word, ocr-image-text).
- `seo`, assistant index and `og` were re-run (188 articles, 6,705 knowledge passages, 31 tests pass): sitemap, feed, `llms*.txt`, `.md` copies, `public/seo/articles/*.html` and share images include them.
