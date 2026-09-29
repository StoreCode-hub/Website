# Website

My live websites and web apps, hosted **free on GitHub Pages**. Every folder is one site — the root `index.html` is a landing page linking to all of them.

## Sites

| Folder | App | Live link |
|---|---|---|
| `LiveCam-Img/` | ImgCam — offline photo to JPEG / PNG / WebP converter (PWA) | [Live site](https://storecode-hub.github.io/Website/LiveCam-Img/) |
| `LiveCam-Webp/` | WebPCam — offline camera that records animated WebP (PWA) | [Live site](https://storecode-hub.github.io/Website/LiveCam-Webp/) |
| `index.html` | Landing page linking to all apps | [Live site](https://storecode-hub.github.io/Website/) |

## Structure

```
Website/
├── README.md                        <- this file
├── index.html                       <- landing page (links to all apps)
├── .github/workflows/               <- auto-deploys everything to GitHub Pages on push
├── LiveCam-Img/                     <- ImgCam: photo -> JPEG / PNG / WebP
│   ├── index.html · manifest.json · sw.js · README.md · icons/
└── LiveCam-Webp/                    <- WebPCam: camera -> animated WebP
    ├── index.html · manifest.json · sw.js · README.md · icons/
```

## How it works

- **Hosting**: GitHub Pages (free, HTTPS, no server — the sites are static files).
- **Deployment**: on every push to `main`, the GitHub Actions workflow uploads the whole
  repo and publishes it. Each app is then live at its folder path, e.g. `/Website/LiveCam-Webp/`.

## Adding a new site

1. Create a new folder, e.g. `MyNextSite/`, and put your `index.html` inside.
2. Add it to the Sites table and Structure section in this README, and to the landing page.
3. Add the folder to the workflow's `paths` trigger.
4. Push - the workflow deploys everything automatically.
