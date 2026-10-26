# Media and README assets

Everything visual in the README lives in `assets/`.

## Screen recording
| File | Size | Use |
|---|---|---|
| `assets/media/demo-short.gif` | about 2 MB, 12 s | Top of the README |
| `assets/media/demo-full.gif` | about 4.5 MB, 24 s | "See it in action" section |
| `assets/media/demo-short.mp4`, `demo-full.mp4` | about 0.6 and 1.2 MB | Sharper versions for the website, social posts and Releases |

The recording is a phone capture of the **PDF to Image Converter**. It was edited so that the file picker (and the folder it showed) is cut out: the story goes from the empty upload box straight to a chosen file. Each step has a caption, the view zooms in on what is happening, and a flash marks the cut.

### Playing the MP4 inline on GitHub
GitHub does not play a video file that is only committed to the repository. To get an inline player in the README, edit the README in the GitHub web editor, **drag `demo-full.mp4` into the text box**, and GitHub gives you a `user-attachments` URL. Paste that URL on its own line.

## Animated SVGs
All are plain SVG with built-in animation (SMIL), so they work in a README, are tiny, and stay sharp on any screen. They are shown through `<img>` tags, which GitHub allows to animate.

| File | What it shows |
|---|---|
| `banner.svg` | Hero banner: typed tagline, floating chips, logo |
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
| `terminal.svg` | A typing quick start |
| `divider.svg` | A dot that travels along a line |

Note: SVG animation is not shown on every site that renders Markdown. GitHub, GitLab and most browsers are fine.

## Static images
`assets/screens/` has cropped screenshots (status bar and floating buttons removed) and `showcase.png`, a static fallback of the animated showcase. `assets/brand/` has the logos. See [BRANDING](BRANDING.md).

## Adding your own recording
1. Capture a short clip, about 10 to 25 seconds, of one flow.
2. Check every frame for anything private: file pickers, notifications, names, the status bar clock.
3. Trim, add captions and zoom, and export a GIF no larger than about 5 MB at around 400 px wide.
4. Keep an MP4 beside it.
