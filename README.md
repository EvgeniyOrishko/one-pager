# One-pager

Mobile landing page for https://helper.makorishko.yoga: a headline in the style of the Instagram stories, a photo, a short description, and a button that opens the Telegram bot.

## Structure

```
index.html              page markup and meta tags (Vite entry)
assets/css/styles.css   styles; colors, fonts and photo controls are variables at the top
assets/img/             hero photo as square WebP in 800/1200/1600 px widths
public/                 copied to dist/ as-is: favicons, web manifest, og.jpg (link preview), robots.txt, sitemap.xml
source-images/          originals: photo and favicon (not referenced by the page)
```

## Editing

- **Bot link:** in `index.html`, replace `your_bot` in `https://t.me/your_bot?start=promo`.
- **Text:** in `index.html`, edit `.title`, `.subtitle`, `.tagline`, `.eyebrow` (headline block) and `.desc` (above the button). Update the `description` / `og:description` / `twitter:description` meta tags to match.
- **Colors and fonts:** variables at the top of `assets/css/styles.css` (`--page-bg`, `--blush`, `--cream`, `--display`, `--sans`).
- **Photo framing:** variables at the top of `styles.css`:
  - `--head-gap`: space between the headline arrow and the top of mom's head. The photo slides up under the headline by a computed `--photo-lift`, so this gap is the same on every screen.
  - `--photo-height`: photo height (share of screen height, with min/max).
  - `--photo-focus-y`: which part of the photo stays in frame when it doesn't fit (0 = top, 1 = bottom).
  - `--photo-zoom`: keep at 1. Above 1 the head-gap formula no longer matches, and the `sizes` attribute on the `<img>` must become `calc(100vw * ZOOM), 520*ZOOMpx`.
  - If you re-crop the photo, update `0.2635` in the `--photo-lift` formula: where the top of the head is, as a share of the image height.
- **Photo file:** put the original in `source-images/`, then regenerate the WebP files (a 2000×2000 square around the people):

```sh
for w in 800 1200 1600; do
  cwebp -q 85 -sharp_yuv -crop 150 400 2000 2000 -resize $w 0 -metadata none \
    source-images/hero-original.jpg -o assets/img/hero-$w.webp
done
```

If the new photo has a different aspect ratio, update the `width`/`height` attributes on the `<img>`.

- **Link preview image:** `public/og.jpg`, 1200×630. It is referenced by its full URL in the meta tags, so keep the file name.

## Development

```sh
npm install
npm run dev      # http://localhost:5174, also reachable from your phone on the same Wi-Fi
npm run build    # production build into dist/
npm run preview  # serve the dist/ build
```

## Deployment

Every push to `master` builds the site and publishes it to GitHub Pages (`.github/workflows/deploy.yml`). The custom domain comes from `public/CNAME`.

One-time setup:
- GitHub → repo **Settings → Pages → Source: GitHub Actions**.
- DNS for `makorishko.yoga`: `CNAME` record `helper` → `evgeniyorishko.github.io`.
- After DNS resolves: **Settings → Pages → Enforce HTTPS**.
