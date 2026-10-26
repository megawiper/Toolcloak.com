<!-- tc:header -->
<sub>[🏠 Toolcloak](../README.md) › [Docs](README.md) › **Advertise, donate and support**</sub>
<!-- /tc:header -->

# Advertise, donate and support

> Sponsor slots, donations and supporters, all static. There is no ad server.

Everything here is static. There is no ad server.

## Sponsor slots
Six places show a sponsor slot (`src/components/SponsorSlot.jsx`, `src/lib/sponsors.js`):

| Slot | Where |
|---|---|
| `home` | Home page |
| `tool` | Every tool page |
| `article` | Guides |
| `category` | Category pages |
| `supporters` | Supporters page |
| `templates` | Templates page |

An empty slot shows a dashed **"Your ad can be here"** box with a **Contact to advertise** button and a link to `/sponsor` (sizes and details).

### Selling one
1. Put the picture, or a short `.mp4`, in `public/sponsors/`.
2. Add one entry to `SPONSORS` (image, video or text).
3. Push.
`until: 'YYYY-MM-DD'` ends it by itself, checked in the visitor's browser, with no rebuild.

### Safety rules (enforced in code and tested)
- The picture or video must be a file **on this site**, so an advertiser's server never sees a visitor.
- Links must be `https`.
- Every sponsor is labelled **Sponsored**.
- Toolcloak may decline ads that are misleading, unsafe or unrelated to what visitors need.

## Contact email: one place
`src/lib/business.js` holds `BUSINESS.email`. Every "Contact to advertise", "I want this" and "Email this request" button is a `mailto:` built from it. While it is empty, the buttons go to `/sponsor#contact`, which says the email is coming soon (no dead links). The same file holds `NEWSLETTER.url` and optional `PRICES` lines.

## Donate
A small "Donate us" button sits under every tool. Tools with a Generate or Convert button show it only after a result; tools that answer by themselves show it at once. Choosing a country (India for now) shows a UPI ID with Copy, a QR drawn in your browser, and a *Pay with UPI app* link. `/donate` has amount chips, a bigger QR and *Download QR*. Settings live in `src/lib/donate.js` (`upiId`, `payeeName`, `note`, `amounts`).

## Supporters
`/supporters` shows a hand-typed list (`src/lib/supporters.js`): add a line `Name, Amount` and commit. The same name twice is added together. Badges: **Supporter**, **Champion** (Rs 100+), **Hero** (Rs 500+). Every place says the list is typed by hand and not connected to UPI.

## Templates
`/templates` has five packs, all "Coming soon". To sell one, set `status: 'live'`, `price` and `buyUrl` (Razorpay, Gumroad or Instamojo). A product counts as live only with a price and an `https` link.

## Custom tool
`/custom-tool` is a request form that opens your mail app with the text filled in, plus a Copy button. Nothing is sent from the page.

## Newsletter
Hidden until `NEWSLETTER.url` is set. Then a card appears on the home page and a link in the footer.


<!-- tc:footer -->
<br/>

---

<div align="center">

<sub><a href="SUPPORT.md">← Support</a> &nbsp;·&nbsp; <a href="BRANDING.md">Branding →</a></sub>
<br/><br/>
<a href="../README.md"><b>← Back to README</b></a> &nbsp;·&nbsp; <a href="README.md">Documentation index</a> &nbsp;·&nbsp; <a href="https://github.com/megawiper/toolcloak.com">megawiper/toolcloak.com</a>

<br/>

<a href="../README.md"><img src="../assets/brand/logo-wordmark.png" height="44" alt="Toolcloak"/></a>

<sub>Free tools, built to be genuinely useful. Your files stay yours.</sub>

</div>
