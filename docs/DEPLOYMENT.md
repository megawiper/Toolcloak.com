# Deployment

Toolcloak is a static site. Cloudflare Pages is the intended host.

## Cloudflare Pages
1. Workers & Pages, then Create, then Pages, then **Connect to Git**, and pick the repository.
2. Build settings:
   - Framework preset: **None**
   - Build command: `npm run build`
   - Build output directory: `out`
   - Environment variable: `NODE_VERSION` = `22`
3. Deploy.

Because there is no `functions/` folder, nothing runs on Cloudflare's servers and no Workers requests are used.

## Headers and redirects
- `public/_headers`: security headers, a separate rule for `/embed/*`, and long cache for `/_next/static/*`.
- `public/_redirects`: the old `/blog` addresses redirect to `/articles` (301).

## Environment variables (optional)
| Variable | Purpose |
|---|---|
| `NODE_VERSION` | Set to `22` |
| `NEXT_PUBLIC_GA_ID` | Overrides the Google Analytics 4 measurement ID |
| `NEXT_PUBLIC_CF_BEACON` | Cloudflare Web Analytics token. Empty means off |

## Changing the domain
Add the new domain under the project's **Custom domains** in Cloudflare, then:
```bash
node scripts/change-domain.js https://your-new-domain.com
npm run build
```
Commit and push.

## Forcing a fresh cache for returning visitors
Bump `VERSION` in `public/sw.js`. Every visitor's cache refreshes on their next visit.

## Pinning dependency versions
Run the **Make package-lock** workflow from the Actions tab. It opens a pull request with `package-lock.json`. Merge it, and builds install exactly the versions that were checked. (GitHub setting needed once: *Settings, Actions, General, Allow GitHub Actions to create and approve pull requests*.)

## After each deploy
- Open Fedra once in a real browser and check the Network tab. The engine was tested in Node.
- Try a PDF, an image and a chain on a phone.
- Check the browser console for blocked items if you changed the Content-Security-Policy.
