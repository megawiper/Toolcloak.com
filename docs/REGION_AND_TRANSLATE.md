<!-- tc:header -->
<sub>[🏠 Toolcloak](../README.md) › [Docs](README.md) › **Region, currency and translate**</sub>
<!-- /tc:header -->

# Region, currency and translate

> Country, currency and page translation.

## Country
The visitor's country is detected in the browser and can be changed in *Settings, Region and currency*.

## Currency
Content is written in rupees. `src/components/RegionEngine.jsx` rewrites rupee amounts in page text (the rupee sign, "Rs", lakh and crore, and chart labels) into the visitor's currency. The original text is remembered, so switching back restores it. Calculator tools handle currency inside `ToolKit.jsx`.

When the visitor's currency is not INR the site loads exchange rates and caches them for 12 hours (`src/lib/region.js`).

**Stay in rupees.** Pages that follow Indian law are listed in `INDIA_TOOLS` in `src/lib/regionData.js` and never convert.

## Translate
Google's page-translate widget, driven by `src/lib/translate.js`. Turn it on site-wide from Settings or the menu, or for one article with the button at the top of the article. The menu, header and footer are marked `translate="no"`. An article translation is dropped when you leave the article.

## Privacy note
Translate loads Google's widget only when the visitor uses it. See [PRIVACY](PRIVACY.md).


<!-- tc:footer -->
<br/>

---

<div align="center">

<sub><a href="PWA_AND_OFFLINE.md">← PWA and Offline</a> &nbsp;·&nbsp; <a href="ARCHITECTURE.md">Architecture →</a></sub>
<br/><br/>
<a href="../README.md"><b>← Back to README</b></a> &nbsp;·&nbsp; <a href="README.md">Documentation index</a> &nbsp;·&nbsp; <a href="https://github.com/megawiper/toolcloak.com">megawiper/toolcloak.com</a>

<br/>

<a href="../README.md"><img src="../assets/brand/logo-wordmark.png" height="44" alt="Toolcloak"/></a>

<sub>Free tools, built to be genuinely useful. Your files stay yours.</sub>

</div>
