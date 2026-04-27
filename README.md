# TARDIS Tracker -- Web / PWA

A Progressive Web App. No Play Store, no App Store. Works in any browser; can be added to the home screen on Android and iOS.

---

## Files

```
web/
├── index.html      -- The entire app (HTML, CSS, JS)
├── manifest.json   -- PWA install config
├── sw.js           -- Service worker (offline support, tile caching)
└── icons/          -- Create these (see below)
    ├── icon-192.png
    └── icon-512.png
```

---

## Icons

Generate two PNG files and drop them in `web/icons/`:

- `icon-192.png` -- 192x192 px
- `icon-512.png` -- 512x512 px

A TARDIS blue square (#003B6F) with a white police box outline works well. Any image editor or online PWA icon generator (e.g. maskable.app) will do it in two minutes.

---

## Hosting options

### GitHub Pages (free, simplest)

1. Push the `web/` folder contents to a GitHub repository.
2. Go to Settings → Pages → Source: Deploy from branch → main / root.
3. Your app is live at `https://yourusername.github.io/reponame/`.

The service worker requires HTTPS, which GitHub Pages provides automatically.

### Netlify (free tier, custom domain support)

1. Drag and drop the `web/` folder onto netlify.com/drop.
2. Done. Custom domain available in settings.

### Any static web host

Upload the contents of `web/` to any host that serves static files over HTTPS. The app has no server-side components.

---

## Refreshing the dataset

Same process as the Android version:

```bash
cd scraper/
python scrape_locations.py
```

Then open `index.html`, find the `const LOCATIONS = [...]` block, and replace the array with the contents of the generated `locations.json`. Redeploy.

A slightly cleaner approach if you expect to update frequently: serve `locations.json` as a separate file and fetch it at startup with `fetch('/locations.json').then(r => r.json())`. The service worker will cache it.

---

## PWA installation

**Android (Chrome):** browser shows an "Add to Home Screen" banner, or use the three-dot menu → Install app.

**iOS (Safari):** tap the Share icon → Add to Home Screen. iOS does not prompt automatically.

Once installed, the app shell works offline. Map tiles are cached as you browse them, so areas you have visited will work offline too.
