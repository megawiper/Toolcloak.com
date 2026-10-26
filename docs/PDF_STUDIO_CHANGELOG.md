<!-- tc:header -->
<sub>[🏠 Toolcloak](../README.md) › [Docs](README.md) › **PDF Studio: change log and feature status**</sub>
<!-- /tc:header -->

# PDF Studio: change log and feature status

> Change log and feature status for PDF Studio.

Spec: Toolcloak_PDF_Studio_Comprehensive_Specification.txt (sections 29 and 30).
Status words follow spec section 27: Implemented = works on supported inputs. Verified = tests passed on documented cases (the person testing marks this). Limited = works with stated limits.
Nothing in this file was run by the assistant: no build, no tests. Please run `npm test` and `npm run build`, then try the checklist at the end of each phase.

## Phase 1 (done before)
Workspace, open / drop, tabs, password dialog, error messages, virtual page rendering, zoom, fit, page navigation, 4 view modes, view rotation, fullscreen, print, shortcuts, recent names.

## Phase 2: search, thumbnails, bookmarks, document info, save foundation
Files: src/components/studio/PdfStudio.jsx (rewritten around Phase 1 code), src/components/studio/pdf/* (engine, ui, useSearch, Thumbs, OutlinePanel, SearchPanel, InfoPanel, SidePanel, SaveAsDialog, panelCss), src/lib/pdfStudioKit.js (more pure functions), src/lib/pdfInspect.js (new), tests/pdfstudio.test.mjs (+13 tests).

| Feature | Status | Notes |
|---|---|---|
| Side panel (Pages / Bookmarks / Search / Info), show/hide, phone drawer | Implemented | closes itself after you pick a page on a phone |
| Page thumbnails | Implemented | drawn only when visible, 2 at a time, freed when off-screen; S/M/L size; shows page labels (i, ii, A-1) when the PDF has them; follows the view rotation |
| Bookmarks (outline) | Limited | jump to page; nested, expand/collapse all; broken targets reported; web links ask first and only http/https/mailto/tel; jumps to the page, not the exact spot on the page |
| Search | Implemented | all pages, match case, whole word, match accents, next/previous (Enter / Shift+Enter), result list with page numbers, yellow highlight on all matches and orange on the current one, progress + cancel, honest message for scans (no text) |
| Search: highlight position | Limited | placed from text width; rotated/vertical text can be slightly off |
| Search comments / bookmarks | Planned | |
| Document info | Implemented | name, size, pages, PDF version (file header), page sizes (A4/Letter names, mixed sizes), password state, linearized, dates, creator/producer |
| Inside the PDF (fonts embedded or not, images, colour spaces, annotations, form fields, XFA, signatures present, attachments, layers, tagged, language, open action, scripts present, page boxes) | Limited | read with pdf-lib; first 3000 pages; says so when truncated; not a security scan |
| Edit title / author / subject / keywords | Implemented | new version, undoable; producer/creator/created date kept; warns first if the PDF has a signature |
| Working copy + Undo / Redo (Ctrl+Z, Ctrl+Shift+Z, Ctrl+Y) | Implemented | every change is re-opened and checked before it replaces the view; keeps the last 15 steps (300 MB cap) |
| Unsaved-changes protection | Implemented | dot on the tab, status bar text, ask before closing a tab / all tabs, browser warning when leaving the page |
| Download (Ctrl+S), Save a copy as… (Ctrl+Shift+S) | Implemented | name check (illegal characters, wrong extension), size, "larger than original" warning, "Choose location…" in Chrome/Edge, Return to document |
| Revert to last saved copy | Implemented | itself undoable |
| File menu, Share (phone share sheet or download) | Implemented | |
| Password-protected files | Limited | open and read; view-only (editing says why) |
| Focus trap + Escape in dialogs, shortcuts help | Implemented | |

Known limits: bookmark jump is page-level; search highlights for rotated text are approximate; Info inspection does not look into password-protected files.

### Try this (about 10 minutes)
1. Open a multi-page PDF. The panel opens on the left with thumbnails; click a thumbnail, scroll, watch the current one follow.
2. Ctrl+F, type a word, Enter. Highlights appear; Enter / Shift+Enter move between matches; toggle Aa / Word / é.
3. Open a scanned PDF and search: you should get the "No searchable text" message.
4. Bookmarks tab on a PDF with an outline (a book, a manual).
5. Info tab: change the Title, Save properties. The tab gets a dot. Ctrl+Z undoes, Ctrl+Shift+Z redoes. Download: the saved file has the new title.
6. Close the tab with the dot: a question appears. Try Revert from the File menu.
7. Open a password PDF: Info says "view only"; no edit works.

## Phase 3 (organise, merge, split, stamps, convert): Tools tab
Implemented (Node-tested in tests/pdfops.test.mjs; UI not yet tried in a live browser): see WORK-STATUS "Session p3". Status: Implemented. Please try each tool once on a real PDF and use Undo to check it.

## Checkpoint 1 (My Space)
- **PDF Studio now saves to My Space.** Every open PDF becomes a project (type "pdf"): saved on open, re-saved 0.8 s after every edit / rename, with a first-page thumbnail. Reopen from My Space > Projects (`/studio/pdf?project=ID`). File menu: "Save to My Space now", "Open My Space". Limit 150 MB per PDF; no older versions are kept above 20 MB. Closing a tab that is saved no longer asks "close without saving?".
- **My Space redesigned** (LibraryClient, LibraryProjects, LibraryUi): tabs Home / Projects / Files / Chains / Starred / History / Manage. Home = "Continue where you left off" (project cards with pictures) + quick start + recent lists. Storage, backup ZIP and export/import live in "Manage".
- **Icons:** Document Scanner glyph, tools without their own glyph borrow the category glyph, Drop Zone suggestions and "Open in studio" buttons now show icons.

## Checkpoint 2 (desktop features)
New files: `pdf/OrganizeDialog.jsx`, `pdf/MarkupLayer.jsx`, `pdf/MarkupBar.jsx`, `pdf/SignDialog.jsx`, `pdf/FormsPanel.jsx`, `pdf/imgUtil.js`, `pdf/featureCss.js`, `src/lib/pdfMarkup.js`; more functions in `src/lib/pdfOps.js`; tests `tests/pdforganize.test.mjs`, `tests/pdfmarkup.test.mjs`.

| Feature | Status | Notes |
|---|---|---|
| **Organize pages** (toolbar "Organize") | Implemented | every page as a picture; click / Ctrl / Shift to select; drag to reorder (or Earlier / Later buttons); rotate, delete, copy, add blank; size slider; nothing changes until "Apply changes"; one undo step. Moving / deleting / rotating keeps the same document (bookmarks, forms stay). Copies and blank pages build a fresh file (warned). |
| **Markup** (toolbar "Markup") | Implemented | Text (size, font, bold, colour; Hindi works as a picture), Highlight, Box, White-out, Black cover, Signature / picture. Move, resize, delete (Delete key). All marks are written in one step ("Add to PDF"), undoable. Works on pages with their own rotation. Hidden while the view is turned (view is reset when Markup opens). |
| **Signature** | Implemented | draw with mouse / finger or type a name (3 styles, 3 inks); transparent PNG; kept in memory only. It is a picture, not a legal digital signature (said in the dialog). |
| **Black cover** | Limited | only hides what you see; text underneath stays in the file (warned in the bar). For real removal use Tools > Shrink hard. |
| **Forms tab** | Implemented | lists text, checkbox, dropdown, list, radio fields; "Fill in the PDF" or "Fill in and flatten". Hindi in a form field is refused with a clear message (standard form fonts). Also in File menu. |
| **Reading themes** (Light / Sepia / Dark / Grey) | Implemented | only how pages look on screen, never the file; remembered. |
| **Present** | Implemented | full screen, one page, arrows / Space / Enter / Backspace, small hover bar, Esc exits and restores your layout. |
| **Start screen** | Implemented | "Merge PDFs" and "Pictures to PDF" without opening a file first. |
| Unsaved-markup protection | Implemented | warning before closing a tab or leaving the page. |

Verified by me: `node --test tests/*.test.mjs` = 239 pass. I also ran the real PdfStudio in headless Chromium (own mini bundler, pdf.js 5 test build): open PDF, markup text + box + apply + undo, signature, organize (reorder + delete + apply), dark theme, present, forms fill, quick merge: all passed. A real bug found this way (pdf-lib minified class names broke form-field detection) is fixed.
NOT run: `next build` (no node_modules / network here). Please run `npm test` and `npm run build`, then try each feature once.
Not done (honest): password protection / encryption (pdf-lib cannot), real redaction, drawing/freehand highlight pen, comments panel.
Note: another edit made at the same time moved the Forms entry into the File menu and renamed the Download button to "Save"; I kept that.

## Checkpoint 5 (speed + Home hero)
- **Studio lag:** pages no longer re-render on every scroll tick (PageView is memoised, scroll position is committed only every ~260 px, layout recomputes only when zoom/mode/size really change). While zooming, the picture is stretched instantly and the sharp redraw waits 160 ms; page pictures are capped at 5 million pixels. Measured in headless Chromium on a 120-page PDF: during Ctrl+wheel zoom, slow frames (>50 ms) went 38 -> 9 and long tasks 532 ms -> 256 ms; fast scrolling long tasks 106 ms -> 0.
- **Home hero redesigned** (HomeClient.jsx, class prefix `hx-`): big headline, large search bar with a Search button, buttons, three proper trust cards (No upload / No signup / Fast), popular-tool chips, and on desktop a small "Image Compressor" preview card. The old small "No signup." text and the stats strip are gone. Articles, guides and every other Home section are untouched, so the long text content stays for AdSense.
- **Home LCP:** the largest paint on Home is the hero text (paragraph on phone, H1 on desktop). It used Sora / DM Sans, which swap in late and move the text. The hero now uses the system font only, so its text paints at once and never jumps. Not measured with real Lighthouse / WebPageTest (not available here): please run PageSpeed on the live site and compare.


<!-- tc:footer -->
<br/>

---

<div align="center">

<sub><a href="MEDIA.md">← Media</a> &nbsp;·&nbsp; <a href="DEVELOPMENT_LOG.md">Development Log →</a></sub>
<br/><br/>
<a href="../README.md"><b>← Back to README</b></a> &nbsp;·&nbsp; <a href="README.md">Documentation index</a> &nbsp;·&nbsp; <a href="https://github.com/megawiper/toolcloak.com">megawiper/toolcloak.com</a>

<br/>

<a href="../README.md"><img src="../assets/brand/logo-wordmark.png" height="44" alt="Toolcloak"/></a>

<sub>Free tools, built to be genuinely useful. Your files stay yours.</sub>

</div>
