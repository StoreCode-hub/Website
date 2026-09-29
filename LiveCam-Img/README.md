# ImgCam — Offline Photo to JPEG / PNG / WebP (Android Browser PWA)

Take a photo with your phone camera and convert it instantly to **JPEG (default), PNG or WebP** — 100% offline, RAM-only.
No server, no upload, no storage of your photos — everything lives in RAM and is gone the moment you clear it or close the app.

## What it does

- Live viewfinder with the **exact ratio crop** you picked (**9:16** default, 1:1, 16:9, 4:3)
- **Formats**: ★ **JPEG** (default) · PNG · WebP (static)
- **Quality**: LOW / ★ MEDIUM / HIGH — applies to JPEG & WebP; PNG is lossless so the control locks itself
- **Output size**: 360p / ★ **480p** / 720p (short edge — 480p is the low-RAM sweet spot for 4 GB phones)
- Instant single-frame conversion via `canvas → toBlob` — no video encoding, minimal RAM (a few MB peak)
- **Single preview rule**: only ONE preview can exist at a time. Making a new one automatically releases the old one. CLEAR (or the trash button) instantly revokes RAM
- Camera ON/OFF that fully stops every track and releases decoder RAM; camera also auto-pauses when you switch apps to save RAM
- Front/back camera switch (selfie mirrored like the viewfinder), live grid overlay, shutter flash
- File name: `imgcam_YYYY-MM-DD_HH-MM-SS.jpg|png|webp` → SAVE button downloads it

## Requirements

- Android phone with Chrome / any modern browser
- First open must be over **HTTPS** (browser rule for camera access — same for every website)

## Install (one time, then forever offline)

1. Upload this whole folder to any free HTTPS static host — GitHub Pages, Netlify Drop (drag & drop), Cloudflare Pages, etc.
   *(Local test: `python3 -m http.server 8000` inside this folder, then open `http://localhost:8000`)*
2. Open the URL in Chrome → allow camera when asked.
3. Chrome menu (⋮) → **Add to Home screen**.
4. From now on, launch it like a real app. Airplane mode is fine — the service worker serves everything from cache.

## RAM design (tuned for 4 GB phones)

- Nothing touches localStorage, IndexedDB, cookies, HTTP cache or disk — settings reset when the app closes (by design)
- One reused canvas is freed (`width = 0`) the instant conversion finishes
- Camera auto-stops when the app goes to background; `video.srcObject = null` releases decoder buffers
- Old Blob URL is revoked before a new preview is created → exactly one preview in RAM at any moment
- 480p default keeps peak RAM under ~10 MB even with the preview on screen

## Troubleshooting

| Problem | Fix |
|---|---|
| "Permission denied" | Address-bar lock icon → Permissions → Camera = Allow, then tap ON |
| "Camera API unavailable" | Open over HTTPS (or localhost), not `file://` |
| Black viewfinder | Another app holds the camera — close it, tap OFF then ON |

## Files

```
imgcam-jpeg-png-webp/
├── index.html      ← whole app (HTML + CSS + JS, zero dependencies)
├── manifest.json   ← PWA identity
├── sw.js           ← offline service worker (cache v1.0.0)
└── icons/          ← app icons (regular + maskable)
```
