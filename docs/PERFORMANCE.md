<!-- tc:header -->
<sub>[🏠 Toolcloak](../README.md) › [Docs](README.md) › **Performance**</sub>
<!-- /tc:header -->

# Performance

> Fast on a cheap phone, on purpose.

Fast on a cheap phone is a goal, not an accident.

## What is already done
- **Tiny main bundle.** Only the header, footer, theme guard and settings applier ship upfront. Everything else is a separate chunk loaded on idle, on first touch, scroll or key press.
- **Menu and search on demand.** The menu drawer loads on first open or hover. The desktop search box loads after idle.
- **Light home page.** The home page reads `home-lite.js` (about 11 KB) instead of the full 146 KB tool list. The full list loads only for visitors who already have History or a Wishlist.
- **System font for the hero**, so the largest paint is never delayed by a font swap.
- **Only three web font families**, and none render-blocking in articles.
- **`content-visibility: auto`** for sections below the first screen.
- **Static glow instead of endless animation** on the tool box, and lighter shadows.
- **No always-on observers** in the region engine while the currency is rupees.
- **`removeConsole`** in production builds.
- **Long caching** for `/_next/static/*` (one year, immutable) and sensible caching for icons, favicons and social images.
- **PDF Studio:** pages are memoised, scroll position is committed every ~260 px, zoom stretches instantly and redraws sharp after 160 ms, page pictures are capped at 5 million pixels.
- **Result files stay the right size.** Watermarking keeps the source image type and steps quality down until the result is no bigger than the original.

## Measuring
Run PageSpeed Insights or WebPageTest after each deploy. Check Core Web Vitals in Search Console. Real-device checks matter more than lab scores.


<!-- tc:footer -->
<br/>

---

<div align="center">

<sub><a href="DEPLOYMENT.md">← Deployment</a> &nbsp;·&nbsp; <a href="SEO.md">SEO →</a></sub>
<br/><br/>
<a href="../README.md"><b>← Back to README</b></a> &nbsp;·&nbsp; <a href="README.md">Documentation index</a> &nbsp;·&nbsp; <a href="https://github.com/megawiper/toolcloak.com">megawiper/toolcloak.com</a>

<br/>

<a href="../README.md"><img src="../assets/brand/logo-wordmark.png" height="44" alt="Toolcloak"/></a>

<sub>Free tools, built to be genuinely useful. Your files stay yours.</sub>

</div>
