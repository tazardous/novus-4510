# Novus 4510 Mathematician — installable web app

Self-contained emulator of the 1976 Novus 4510. No build step, no dependencies,
no network calls at runtime.

```
index.html             emulator, mobile metadata, viewport scaling, SW registration
manifest.webmanifest   app name, icons, standalone display
sw.js                  cache-first service worker (offline)
icon-192.png
icon-512.png
```

## Deploy to GitHub Pages

```bash
gh repo create novus-4510 --public --source=. --remote=origin --push
```

Then in the repo: **Settings → Pages → Source: Deploy from a branch → main / (root)**.

Or from the CLI:

```bash
gh api -X POST repos/:owner/novus-4510/pages \
  -f 'source[branch]=main' -f 'source[path]=/'
```

Live within a minute or two at `https://<user>.github.io/novus-4510/`.

Everything is relative-pathed, so the project subdirectory works fine — no need
for a custom domain or a root-level deploy.

## Install on the phone

1. Open the Pages URL in Chrome on the S22+.
2. Menu (⋮) → **Add to Home screen** → Install.
3. Launches standalone: no address bar, own icon in the app drawer, works in
   airplane mode after the first load.

## Updating it

Edit, bump `CACHE` in `sw.js` to `novus4510-v2`, push. The phone picks up the new
version on next launch. Without the version bump the old cache wins and nothing
appears to change.

## Alternatives considered

| Route | Effort | Result |
|---|---|---|
| PWA on GitHub Pages | minutes | icon, offline, full screen, instant updates |
| Azure Static Web Apps | ~15 min | same, plus a custom domain and your own CI |
| Save the HTML, open from Files | seconds | works, but no icon and no full-screen mode |
| Capacitor or Cordova wrapper | hours | a real `.apk` to sideload |
| .NET MAUI Blazor Hybrid | a day | a real app, but the engine has to be ported to C# first |

The wrapper routes only pay off if you want Play Store distribution or native
APIs. Neither applies here.

## Notes

- Portrait-locked, scales to fill the phone width, short haptic tick per key.
- Layout is a fixed 336 px design scaled by transform, so proportions stay exact
  at any screen size.
- The stack inspector below the calculator shows X, Y, Z, memory and the lift
  flag — useful for comparing against the physical unit key for key.
