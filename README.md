# PS-1 Pocket Synth — Ship It Guide

Your synth, packaged three ways: test locally, install free as a PWA, and ship
to the App Store with Capacitor. Work through the stages in order — each one is
a working checkpoint.

Everything below runs on your Mac in VS Code / Claude Code.

---

## Stage 1 — Run it locally (2 minutes)

```bash
cd ps1-app
npx serve www
```

Open the printed URL. This is your dev loop: edit `www/index.html`, refresh.

---

## Stage 2 — Free hosted PWA (30 minutes)

Host the `www/` folder anywhere with HTTPS and the app becomes installable on
any phone from Safari/Chrome ("Add to Home Screen") — full screen, custom icon,
works offline via the service worker.

GitHub Pages route (fits your existing workflow):

```bash
cd ps1-app
git init && git add . && git commit -m "PS-1 v1"
# create a repo on github.com, then:
git remote add origin https://github.com/YOURNAME/ps1-synth.git
git push -u origin main
```

On GitHub: Settings → Pages → deploy from branch → `main`, folder `/ (root)`.
Your synth will be live at `https://YOURNAME.github.io/ps1-synth/www/`.

Why do this even if the App Store is the goal: it's your free marketing demo.
"Try it in your browser" converts way better than "trust me, buy it."

---

## Stage 3 — App Store via Capacitor

### Prerequisites
- Xcode from the Mac App Store (free, big download)
- Apple Developer Program: https://developer.apple.com — $99/year (required to ship)

### One-time setup

1. Edit `capacitor.config.json` — change `com.YOURNAME.ps1synth` to your own
   reverse-domain ID (e.g. `com.calebmakes.ps1synth`). Pick once, never change.

2. ```bash
   cd ps1-app
   npm install
   npx cap add ios
   npx cap sync ios
   npx cap open ios
   ```

3. In Xcode: select the project → Signing & Capabilities → set your Team
   (your developer account). Plug in your iPhone, pick it as the target, press ▶.
   **Your synth is now a real app on your phone.** This works even before the
   $99 account (free account = app expires after 7 days).

4. The synth's RECORD SAMPLE button uses the mic (`getUserMedia`), which iOS
   requires an explanation for. In Xcode, open `ios/App/App/Info.plist` and
   add a `Privacy - Microphone Usage Description` entry (e.g. "Used to record
   your voice as a synth sample.") — without it, tapping RECORD SAMPLE will
   silently fail on-device.

### Each release after code changes

```bash
npx cap sync ios   # copies www/ into the iOS project
```
Then build in Xcode.

### Shipping
1. appstoreconnect.apple.com → New App → fill in name, screenshots, description
2. Xcode → Product → Archive → Distribute App
3. Submit for review (typically 1–3 days)

App Store description tip: lead with what it does in one breath —
"A pocket synthesizer and drum machine. Play, sequence, record, mix. No account,
no ads, works offline."

---

## Monetization — the honest version

**What works for small music apps:**

- **Paid up front, $2.99–$4.99** — the classic for instrument apps. Simple,
  honest, zero extra code. Music-app buyers are unusually willing to pay
  small amounts for quality toys. Start here.
- **Free + one unlock (IAP ~$3.99)** — free tier: 4 patterns, 2 save slots;
  unlock: all grooves, all slots, future sound packs. Better funnel, but
  requires implementing StoreKit via the Capacitor IAP plugin — save this
  for v2 once you have downloads.
- **Never ads.** An ad interrupting an instrument kills it, and the revenue
  at small scale is pennies.

**Apple's cut:** 15% for small developers (under $1M/yr — that's you), so a
$3.99 sale nets ~$3.39.

**Honest expectations:** the music-toy category is crowded, and most small
apps make modest money — think "nice freelance side line," not passive-income
rocket. Your realistic edge is content around it: you literally have an
AI-video pipeline. "I built and shipped a synth app with AI" is exactly the
kind of video that finds an audience, and the app link in the description is
the monetization. The app and the story about the app sell each other.

**Suggested sequence:** Stage 2 this week (free, momentum), Stage 3 device
install next (huge morale moment), then decide on the $99 based on whether
you're still excited. Ship at $2.99, learn, iterate.

---

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
└── README.md             ← this file
```
