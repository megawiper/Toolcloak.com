# Contributing to Toolcloak

Thanks for wanting to help. Small fixes and big ideas are both welcome.

## Ways to help
- 🐞 **Report a bug** with the bug template
- 💡 **Suggest a tool** with the feature template
- ✍️ **Improve a guide:** fix a typo, add a clearer example, check a number
- 🌍 **Test on your device:** different browsers and phones find different problems (camera, touch, voice and sync especially)
- 🔐 **Security issues:** follow [SECURITY.md](SECURITY.md), never a public issue

## Set up
```bash
git clone https://github.com/<you>/<repo>.git
cd <repo>
npm install            # Node 22 or newer
npm run dev            # http://localhost:3000
```
Before opening a pull request:
```bash
npm run build                    # prebuild generators run automatically
node --test tests/*.test.mjs     # full test suite
```
Do not edit generated files by hand (see [docs/PROJECT_STRUCTURE.md](docs/PROJECT_STRUCTURE.md)). Change the source and rebuild.

## Before you start
1. Search existing issues so we do not duplicate work.
2. For anything big, open an issue first and describe what you want to build.
3. Read the [Code of Conduct](CODE_OF_CONDUCT.md).

## Proposing a new tool
A good request answers:
- What problem does it solve, and for whom?
- What would someone search to find it?
- What goes in, and what comes out?
- Can it run fully in the browser?
- Which of the 17 categories does it belong to?

How to build one: [docs/ADDING_A_TOOL.md](docs/ADDING_A_TOOL.md).

## Writing or editing a guide
Guides live as plain-data files in `src/content/articles/`. How: [docs/ADDING_AN_ARTICLE.md](docs/ADDING_AN_ARTICLE.md).
- Start from a real question people have.
- Link it to the tool it explains.
- Use real worked numbers.
- Do not overlap an existing guide.

## Ground rules
- **Privacy first.** A tool must not send the user's input anywhere. If a tool truly needs the network, say so on its page.
- **No outside CDNs.** Add libraries through `scripts/copy-vendor.js`.
- **Use the kits.** `TextTool`, `CalcTool` and `LabKit` give a tool History, Artifacts, Chains, Share link and search for free.
- **No emoji in the interface** (the emoji and symbol picker tools are the exception). Use plain arrows and tick marks inside checklists.
- **Accessible by default:** labels, contrast, keyboard use, `aria-label="Breadcrumb"` on breadcrumb navs.

## Pull request checklist
- [ ] The change does one thing
- [ ] `npm run build` passes and `node --test tests/*.test.mjs` passes
- [ ] Tested on mobile and desktop, in light and dark
- [ ] No tool input is sent anywhere it should not be (watch the "bytes uploaded" pill)
- [ ] Docs updated if behaviour changed
- [ ] `CHANGELOG.md` updated

## Licensing of contributions
By contributing you agree that your contribution may be used under the project's licence. <!-- ADD your licence name here once you pick one. -->
