# Region, currency and translate

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
