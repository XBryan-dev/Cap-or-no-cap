# Cap or No Cap

A small installable web app / PWA — spot the real Cameroonian slang, proverbs
and facts from the convincing fakes before the timer runs out.

## Files

```
index.html            the app itself
manifest.webmanifest   makes it installable ("Add to Home Screen")
sw.js                  service worker — offline support + the update flow
icons/                 app icons (generated, monogram mark)
```

## Deploying to GitHub Pages

1. Create a new GitHub repo (or use an existing one) and add all of these
   files to it, keeping the folder structure exactly as-is (icons/ has to
   stay a subfolder next to index.html).
2. Push to GitHub.
3. In the repo: **Settings → Pages → Source**, pick the branch (usually
   `main`) and root folder, then save.
4. GitHub gives you a URL like:
   `https://yourusername.github.io/your-repo-name/`
   That's the live link — open it on your phone and you'll get an
   "Add to Home Screen" / install prompt like a real app.

That's it — no build step, no server, it's fully static.

## How the "Update available" banner works

`sw.js` caches the app so it works offline and loads instantly. The very
first line of that file is:

```js
const VERSION = 'v1.0.0';
```

Whenever you make a change and push it to GitHub Pages:

1. **Bump that version string** (e.g. `v1.0.1`). This is the only thing
   that has to change for the update system to notice — it's how the
   browser knows there's something new to fetch.
2. Anyone who already has the app open (or installed) will automatically
   get the "Update available" banner next time they open it or bring it
   to the foreground.
3. They tap **Update now** → it shows a brief loading spinner → the app
   reloads on the new version. If they ignore it, they keep using the
   old cached version until they do.

It's also worth bumping `APP_VERSION` near the top of the `<script>` in
`index.html` (just the version shown in the corner of the home screen) —
that's cosmetic only and doesn't affect the update logic.

## Notes

- Opening `index.html` directly from your computer (double-clicking the
  file) will run the game fine, but the service worker won't register
  (browsers block that on `file://`) — so install/update/offline only
  work once it's actually hosted somewhere like GitHub Pages.
- The best-score is stored per-device with `localStorage`, so it won't
  sync between your phone and desktop.
- To change the app icon, replace the PNGs in `icons/` (keep the same
  filenames and sizes: 192×192, 512×512, plus the 180×180 apple touch
  icon and 32×32 favicon).
