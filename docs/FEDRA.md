<!-- tc:header -->
<sub>[🏠 Toolcloak](../README.md) › [Docs](README.md) › **Fedra**</sub>
<!-- /tc:header -->

# Fedra

> The assistant with no AI server.

![fedra](../assets/fedra-chat.svg)

Fedra is Toolcloak's assistant. There is a floating **Assistant** button on every page and a full page at `/assistant`. You type a task in plain words and may drop files. Fedra works out the steps, shows the plan in plain language, asks before risky steps, runs them in the background and hands over the result.

> **Every fact or number comes from a tool, a calculator function or the site's own text.** There is no language model, no server and no paid API. Nothing is sent anywhere.

## What Fedra can do
| Ability | How it works |
|---|---|
| **Run multi-step jobs** | Splits "then / after that / commas / numbered lists" into steps, validates the plan with the real chain rules, saves it as a normal chain and runs it with the chain runner |
| **Understand first** | The plan card shows your files and what each step will really do ("Make the file 70% smaller"), and asks any missing value (watermark text, format, size, pages, angle, password) with tap-to-answer options plus a box for your own |
| **Hinglish verbs** | *phir*, *isme*, *uske baad* become steps |
| **Answer how-to questions** | Searches every tool guide and article section (BM25 with typo tolerance and synonyms), picks the best sentences, removes duplicates and links the sources. If the guides do not really cover it, Fedra says so and suggests the closest tools |
| **Do maths** | Equations (linear, quadratic, higher, systems), derivatives, integrals, limits, statistics, gcd and lcm, primes, factorials, nCr, determinants and plots, with exact steps, an SVG graph and a button to the matching tool. A safe arithmetic parser, no `eval` |
| **Read files** | A text file is read: its words are the data, or the instructions if you say "do what is written". A file with no instruction gets a "what should I do?" card |
| **Remember** | Your last watermark text and last job ("same again", "same watermark as before") |
| **Vary answers** | Each answer carries a token (`FD-XXXXXX`) that seeds how it is composed. Regenerate gives a different version of the same site facts |

## Covers every tool, automatically
`scripts/generate-assistant-index.js` runs in `prebuild` and writes `public/assistant/index.json` (every tool: name, descriptions, category, in and out kinds, search terms, readable options), `knowledge.json` (every guide step, tip, FAQ, tool description and article section, as passages), `report.json` and `ASSISTANT-REPORT.md`. A new tool, guide or article is picked up on the next build.

`ASSISTANT-REPORT.md` lists per tool whether it is **runnable**, **open-only**, or runnable with a **gap** (options not readable yet, so it runs with defaults). To close a gap, add the tool's options to `hand-params.mjs` or write them as a `FIELDS` list.

## Engine files (`src/lib/assistant/`)
`text.mjs` (stemmer, typo tolerance) · `dictionary-en.mjs` (all English words and messages) · `synonyms.mjs` · `search.mjs` · `tasks.mjs` · `params.mjs` · `planner.mjs` · `asks.mjs` · `hinglish.mjs` · `calc.mjs` · `math.mjs` · `compare.mjs` · `answer.mjs` · `studios.mjs` · `hand-params.mjs` · `index.mjs`. Plain JavaScript with no React, so it is tested in Node.

## Lazy and light
Only a launcher button loads after the page is ready. The chat, engine and data load when you tap it, and the knowledge file (about 3 MB, 1 MB gzipped) loads only when you ask a how-to question. Fedra's JSON files are cached for an hour and warmed when you touch the button.

## Privacy
- The engine files contain no `fetch`, beacon or analytics call (a test checks this).
- The panel only loads static files from this site.
- No analytics event carries what you typed.
- Uploaded files stay in memory. Only results can be saved, through the normal consent logic.

## Known limits
- English dictionary only (Hinglish verbs are an addition).
- Rule based, so unusual phrasing can miss. Fedra then asks a question with options or suggests tools.
- Options are mapped only for tools whose options are readable.
- A plan can join files at the start or as a joining step, not insert a file in the middle.
- A page-bound tool that needs a click on something other than its main button can fail in the background. The error offers *Do this step myself*.

## Idea: Phase 2 (not built)
An opt-in "Turn on smart mode" button with a download-size notice and a WebGPU and memory check, using a small in-browser model whose only job is to turn messy English into the same plan format, validated against real tools, falling back to the rule engine on anything invalid. It would never produce facts or numbers.

## Tests
`npm test` runs the acceptance tests against the real index and the real chain rules.


<!-- tc:footer -->
<br/>

---

<div align="center">

<sub><a href="TOOL_CHAINS.md">← Tool Chains</a> &nbsp;·&nbsp; <a href="GUIDES.md">Guides →</a></sub>
<br/><br/>
<a href="../README.md"><b>← Back to README</b></a> &nbsp;·&nbsp; <a href="README.md">Documentation index</a> &nbsp;·&nbsp; <a href="https://github.com/megawiper/toolcloak.com">megawiper/toolcloak.com</a>

<br/>

<a href="../README.md"><img src="../assets/brand/logo-wordmark.png" height="44" alt="Toolcloak"/></a>

<sub>Free tools, built to be genuinely useful. Your files stay yours.</sub>

</div>
