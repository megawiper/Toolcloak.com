# My Space and device sync

## My Space (`/library`)
Your home base on this device.

| Tab | What is in it |
|---|---|
| **Home** | *Continue where you left off* (project cards with pictures), quick start, recent lists |
| **Projects** | Everything you made in a Studio: open, rename, duplicate, delete. Search and filter by type, 20 per page |
| **Files** | Saved results (Artifacts) |
| **Chains** | Your chains |
| **Starred** | Your wishlist |
| **History** | What you entered and what you got |
| **Manage** | Storage use, **Backup all (ZIP)**, **Restore**, export and import, and "keep my projects" (asks the browser to protect storage from automatic clearing) |

Projects are saved on open and re-saved shortly after every edit, with a first-page thumbnail. PDFs are limited to 150 MB each, and older versions are not kept. Models in the 3D viewer are not saved (only settings).

## Artifacts, History and Wishlist
- Every result a tool makes is stored automatically in IndexedDB, once. The file you upload is never stored.
- History records the picked file name, keeps the result, and updates one entry per run. It needs your permission, and Toolcloak asks again (in a tool, or on the History page) until you decide.
- Export and import history, saved results and wishlist as JSON. Import merges and overwrites nothing.

## Sync between your devices (`/sync`)
Move projects, chains, history and saved files between your phone and your laptop with a QR code.

- **No account and no server in between.** The two devices connect directly (WebRTC). See `src/lib/syncRtc.js` and `syncTransfer.js`.
- The scanner uses the browser's QR detector, or `jsQR` where that is missing.
- Both devices need to be online at the same moment while they sync.
