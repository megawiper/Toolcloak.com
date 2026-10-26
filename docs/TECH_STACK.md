<!-- tc:header -->
<sub>[🏠 Toolcloak](../README.md) › [Docs](README.md) › **Tech stack**</sub>
<!-- /tc:header -->

# Tech stack

> The framework, the libraries and why they are vendored.

## Core
| | |
|---|---|
| Framework | Next.js 14 (App Router, `output: 'export'`) |
| UI | React 18 |
| CSS | Tailwind CSS 3, PostCSS, Autoprefixer, plus hand-written CSS for studios and articles |
| Node | 22 or newer (`.nvmrc`, `engines`) |
| Hosting | Cloudflare Pages (static) |

## Libraries and what they power
| Library | Used for |
|---|---|
| `pdf-lib` | Creating and editing PDFs: merge, split, markup, forms, metadata |
| `pdfjs-dist` 3.11 | Rendering PDF pages, thumbnails, search, text extraction |
| `tesseract.js` + language packs | OCR in 13 languages: Arabic, Bengali, German, English, French, Gujarati, Hindi, Marathi, Punjabi, Russian, Spanish, Tamil, Telugu |
| `xlsx` | Excel and CSV import and export (Sheets, Data Studio, converters) |
| `jszip` | ZIP creation and extraction, project backups |
| `three` r128 | 3D Viewer and Print |
| `lamejs` | MP3 encoding |
| `heic2any` | HEIC to JPG |
| `jsbarcode`, `jsqr` | Barcode drawing, QR scanning |
| `js-beautify`, `js-yaml`, `terser` | Code formatting, YAML, JS minifier |
| `mermaid` | Diagram tools |

QR codes are drawn by Toolcloak's own code (`src/lib/qr.js`).

## Why vendored
Libraries are copied from npm into `public/vendor/` at build time (`scripts/copy-vendor.js`). The site never loads code from an outside CDN, which keeps visitors' requests on one origin and lets the service worker cache everything for offline use.

## Size
About 113,000 lines of JavaScript and JSX across `src`, with 95 modules in `src/lib`, and 17 test files.

## Fonts
Three families only (Sora, DM Sans, DM Mono), with system fonts for the hero text and articles so first paint is never blocked by a font.


<!-- tc:footer -->
<br/>

---

<div align="center">

<sub><a href="ARCHITECTURE.md">← Architecture</a> &nbsp;·&nbsp; <a href="PROJECT_STRUCTURE.md">Project Structure →</a></sub>
<br/><br/>
<a href="../README.md"><b>← Back to README</b></a> &nbsp;·&nbsp; <a href="README.md">Documentation index</a> &nbsp;·&nbsp; <a href="https://github.com/megawiper/toolcloak.com">megawiper/toolcloak.com</a>

<br/>

<a href="../README.md"><img src="../assets/brand/logo-wordmark.png" height="44" alt="Toolcloak"/></a>

<sub>Free tools, built to be genuinely useful. Your files stay yours.</sub>

</div>
