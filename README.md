# Website

My live websites and web apps, hosted **free on GitHub Pages**. Every folder is one site.

## Sites

| Folder | App | Live link |
|---|---|---|
| `LiveCam-Img/` | ImgCam — offline photo to JPEG / PNG / WebP converter (PWA) | [Live site](https://storecode-hub.github.io/Website/) |

## Structure

```
Website/
├── README.md                        <- this file
├── .github/workflows/               <- auto-deploys to GitHub Pages on every push
└── LiveCam-Img/                     <- ImgCam app (self-contained)
    ├── index.html                   <- the entire app (HTML + CSS + JS)
    ├── manifest.json                <- PWA manifest (installable app)
    ├── sw.js                        <- service worker (offline support)
    ├── icons/                       <- app icons (standard + maskable)
    └── README.md                     <- full app documentation
```

## How it works

- **Hosting**: GitHub Pages (free, HTTPS, no server needed - the site is static files).
- **Deployment**: on every push to `main`, the GitHub Actions workflow uploads the
  `LiveCam-Img/` folder and publishes it to the live URL above. No manual steps.

## Adding a new site

1. Create a new folder, e.g. `MyNextSite/`, and put your `index.html` inside.
2. Update the Sites table and Structure section in this README.
3. Add the folder to the workflow's `paths` trigger and change the artifact `path`.
4. Push - the workflow deploys everything automatically.
