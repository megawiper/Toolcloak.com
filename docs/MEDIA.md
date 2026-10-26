<!-- tc:header -->
<sub>[🏠 Toolcloak](../README.md) › [Docs](README.md) › **Media and README assets**</sub>
<!-- /tc:header -->

# Media and README assets

Everything visual in the README lives in `assets/`.

## 🎬 Demo recording

| File | Size | Use |
|---|---|---|
| `assets/media/demo-desktop.gif` | about 4 MB, 9 s, 960 px wide | Top of the README (the only place the demo is shown) |
| `assets/media/demo-desktop.mp4` | about 1.6 MB, 9 s, 1920x1080 | The sharp version, linked from the GIF, for the website, social posts and Releases |
| `assets/media/demo-poster.jpg` | about 90 KB | A still frame for link previews |

The recording is a phone capture of the **PDF to Image Converter**, laid out for **desktop**: a 16:9 canvas with the phone on the right and live captions on the left.

What was edited:

- ✂️ The **file picker (and the folder it showed) is cut out**. The story goes from the empty upload box straight to a chosen file.
- ⏩ Slow parts (choosing quality, waiting for the conversion) are sped up. The moment the pill says **0 bytes uploaded** keeps its natural speed.
- 📝 A five-step list on the left follows the action and ticks each step off.
- 🧹 The phone status bar (clock, network speed, battery) is cropped away.

### ▶️ Playing the MP4 inline on GitHub

GitHub does not play a video file that is only committed to the repository. To get an inline player in the README, edit the README in the GitHub web editor, **drag `demo-desktop.mp4` into the text box**, and GitHub gives you a `user-attachments` URL. Paste that URL on its own line.

## ✨ Animated SVGs

All are plain SVG with built-in animation (SMIL), so they work in a README, stay small, and stay sharp on any screen. They are shown through `<img>` tags, which GitHub allows to animate.

| File | What it shows |
|---|---|
| `banner.svg` | Hero banner: typed tagline, floating chips, orbiting logo |
| `highlights.svg` | Six feature cards that rise in one by one |
| `showcase-animated.svg` | Three floating phone screens on a moving gradient |
| `marquee.svg` | Two rows of tools scrolling in opposite directions |
| `stats.svg` | Numbers tiles |
| `categories.svg` | All 17 categories as bars that grow |
| `studios.svg` | The Studios as bouncing tiles |
| `chain-flow.svg` | Merge, compress, watermark with a travelling dot |
| `fedra-chat.svg` | An example chat with Fedra |
| `privacy-flow.svg`, `privacy-pill.svg` | File to browser to result, and the "0 bytes uploaded" pill dropping in |
| `voice-wave.svg` | Listening wave with example commands |
| `architecture.svg` | Static files in, your files never out |
| `terminal.svg` | A typing quick start for `megawiper/toolcloak.com` |
| `divider.svg` | A dot that travels along a line |

> [!NOTE]
> SVG animation is not shown on every site that renders Markdown. GitHub, GitLab and most browsers are fine.

## 🖼️ Static images

`assets/screens/` has cropped screenshots (status bar and floating buttons removed) and `showcase.png`, a static fallback of the animated showcase. `assets/brand/` has the logos. See [BRANDING](BRANDING.md).

## 🎥 Adding your own recording

1. Capture a short clip, about 8 to 15 seconds, of one flow.
2. Check every frame for anything private: file pickers, notifications, names, the status bar clock.
3. Trim, add captions and lay it out on a 16:9 canvas so it reads well on a desktop README.
4. Export a GIF no larger than about 5 MB at around 960 px wide, and keep an MP4 beside it.


<!-- tc:footer -->
<br/>

---

<div align="center">

<sub><a href="ROADMAP.md">← Roadmap</a> &nbsp;·&nbsp; <a href="PDF_STUDIO_CHANGELOG.md">PDF Studio Changelog →</a></sub>
<br/><br/>
<a href="../README.md"><b>← Back to README</b></a> &nbsp;·&nbsp; <a href="README.md">Documentation index</a> &nbsp;·&nbsp; <a href="https://github.com/megawiper/toolcloak.com">megawiper/toolcloak.com</a>

<br/>

<a href="../README.md"><img src="../assets/brand/logo-wordmark.png" height="44" alt="Toolcloak"/></a>

<sub>Free tools, built to be genuinely useful. Your files stay yours.</sub>

</div>
