<!-- tc:header -->
<sub>[🏠 Toolcloak](../README.md) › [Docs](README.md) › **Testing**</sub>
<!-- /tc:header -->

# Testing

> Node's built-in runner, with no extra framework.

Tests use Node's built-in runner. No extra framework.

```bash
npm test                          # Fedra acceptance tests
node --test tests/*.test.mjs      # everything
```

## What is covered
| File | Covers |
|---|---|
| `assistant.test.mjs` | Fedra planning against the real index and real chain rules: typo tolerance, size and format parsing, maths, out-of-scope replies, bridge converters, coverage of every tool slug, every tool findable by its own name, and **no network or analytics calls inside the engine** |
| `business.test.mjs` | Sponsor safety rules (own-site media, https links, "Sponsored" label), supporters, templates |
| `pdfops.test.mjs`, `pdforganize.test.mjs`, `pdfmarkup.test.mjs`, `pdfstudio.test.mjs` | PDF operations, organise, markup and studio helpers |
| `data.test.mjs`, `sheets.test.mjs`, `docs.test.mjs`, `docx.test.mjs` | Data Studio, Sheets engine, Docs kit, Word export |
| `design.test.mjs`, `graph.test.mjs`, `mesh3d.test.mjs` | Design kit, geometry and graphing, 3D mesh tools |
| `studios.test.mjs`, `usage.test.mjs`, `resultcard.test.mjs`, `sendgate.test.mjs` | Studio registry, the upload meter, result cards, send gating |

## What tests cannot tell you
Several features need a real browser: camera, touch, WebRTC sync, voice, and the visual feel of the studios. After a change that touches them, try them by hand on a phone and a laptop.

## Adding tests
Put `something.test.mjs` in `tests/`. Pure logic belongs in `src/lib` so it can be loaded from Node with `tests/helpers/loadLib.mjs`.


<!-- tc:footer -->
<br/>

---

<div align="center">

<sub><a href="SCRIPTS.md">← Scripts</a> &nbsp;·&nbsp; <a href="DEPLOYMENT.md">Deployment →</a></sub>
<br/><br/>
<a href="../README.md"><b>← Back to README</b></a> &nbsp;·&nbsp; <a href="README.md">Documentation index</a> &nbsp;·&nbsp; <a href="https://github.com/megawiper/toolcloak.com">megawiper/toolcloak.com</a>

<br/>

<a href="../README.md"><img src="../assets/brand/logo-wordmark.png" height="44" alt="Toolcloak"/></a>

<sub>Free tools, built to be genuinely useful. Your files stay yours.</sub>

</div>
