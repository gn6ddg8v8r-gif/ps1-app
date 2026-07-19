# Project context for Claude

This file governs `ps1-app/` only. It overrides the parent
`/Users/cjstech/Downloads/CLAUDE.md`, which describes an unrelated project
(an Economic Seasons investing dashboard) and does not apply here.

## What this project is

PS-1 Pocket Synth — a synthesizer and groovebox web app, shipped three ways:
1. Local dev (`npx serve www`)
2. Free hosted PWA (installable from a browser, works offline via service worker)
3. iOS App Store app, wrapped with Capacitor

The entire app lives in `www/` (`index.html`, manifest, service worker,
icons). That folder is what ships in every packaging method — treat it as
the single source of truth for the app's functionality.

## Project layout

```
ps1-app/
├── www/                  ← the entire app (this is what ships)
│   ├── index.html        ← the synth (PWA-wired)
│   ├── manifest.webmanifest
│   ├── sw.js             ← offline caching
│   └── icons (192/512/apple-touch)
├── icon-1024.png         ← App Store listing icon
├── capacitor.config.json
├── package.json
└── README.md              ← full "Ship It Guide" — read for release steps
```

## Guardrails for feature work

- Keep the app dependency-light and framework-free in `www/` — no build
  step, so `npx serve www` always works as the dev loop and the PWA stays
  installable offline.
- `www/index.html` is wired for PWA installability (manifest + service
  worker). Don't break offline caching (`sw.js`) when editing.
- `capacitor.config.json`'s `appId` is a placeholder
  (`com.YOURNAME.ps1synth`) until the owner picks a real reverse-domain ID —
  flag it if it's still a placeholder when Capacitor/App Store steps come up.
- See `README.md` for the full release pipeline (local → PWA → App Store)
  and monetization notes.

## Owner's skill level

Learning as they go. Explain changes in plain language, prefer small
verifiable steps, and after changes say exactly how to check the result
(e.g. reload the browser tab, or `npx cap sync ios` + rebuild in Xcode).
