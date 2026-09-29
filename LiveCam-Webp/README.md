# WebPCam — Offline Animated WebP Camera (Android Browser PWA)

Record your phone camera straight to **animated WebP**, 100% offline, RAM-only.
No server, no account, no storage of your captures — everything lives in RAM and is gone the moment you clear it or close the app.

## What it does

- Live viewfinder with the **exact ratio crop** you picked (9:16 default, 1:1, 16:9, 4:3)
- **Duration**: 1 – 20 seconds (default 5 s) · **FPS**: 10 / 12 / 15 / 20 / 24 (default 12)
- **Output size**: 360p / **480p** / 720p (short edge — 480p is the low-RAM sweet spot for 4 GB phones)
- Real **animated WebP encoding on-device** using WebCodecs VP8 (all keyframes) + a native RIFF/ANMF/VP8X muxer written in JS — no libraries, no internet
- **Single preview rule**: only ONE preview can exist at a time. Making a new one automatically releases the old one. CLEAR (or the trash button) instantly revokes RAM
- Camera ON/OFF that fully stops every track and releases decoder RAM; camera also auto-pauses when you switch apps to save RAM
- Front/back camera switch, live grid overlay, shutter flash, encoding progress %
- File name: `webpcam_YYYY-MM-DD_HH-MM-SS.webp` → SAVE button downloads it

## Requirements

- Android phone with **Chrome 94+** (needed for WebCodecs VP8 encoding)
- First open must be over **HTTPS** (browser rule for camera access — same for every website)

## Install (one time, then forever offline)

1. Upload this whole folder to any free HTTPS static host — GitHub Pages, Netlify Drop (drag & drop), Cloudflare Pages, etc.
   *(Local test: `python3 -m http.server 8000` inside this folder, then open `http://localhost:8000`)*
2. Open the URL in Chrome → allow camera when asked.
3. Chrome menu (⋮) → **Add to Home screen**.
4. From now on, launch it like a real app. Airplane mode is fine — the service worker serves everything from cache.

## RAM design (tuned for 4 GB phones)

- Nothing touches localStorage, IndexedDB, cookies, HTTP cache or disk — settings reset when the app closes (by design)
- Frames are held **compressed** (VP8 bytes) while encoding, then muxed into a single Blob — no raw video ever sits in memory
- Canvas is freed (`width = 0`) the instant encoding ends
- Camera auto-stops when the app goes to background; `video.srcObject = null` releases decoder buffers
- Old Blob URL is revoked before a new preview is created → exactly one preview in RAM at any moment

## Troubleshooting

| Problem | Fix |
|---|---|
| "Permission denied" | Address-bar lock icon → Permissions → Camera = Allow, then tap ON |
| "Camera API unavailable" | Open over HTTPS (or localhost), not `file://` |
| "VP8 encoding not supported" | Update Chrome to 94+ |
| Black viewfinder | Another app holds the camera — close it, tap OFF then ON |

## Files

```
webpcam-animated-webp/
├── index.html      ← whole app (HTML + CSS + JS, zero dependencies)
├── manifest.json   ← PWA identity
├── sw.js           ← offline service worker (cache v1.0.0)
└── icons/          ← app icons (regular + maskable)
```
