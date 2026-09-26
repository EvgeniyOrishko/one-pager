# One-pager

Mobile landing page: full-screen photo, short description, and a button that opens the Telegram bot.

## Structure

```
index.html              page markup (Vite entry)
assets/css/styles.css   styles
assets/img/             optimized WebP images served by the page
public/                 favicons + web manifest, copied to dist/ as-is
source-images/          originals: photo and favicon (not referenced by the page)
```

## Editing

- **Bot link:** in `index.html`, replace `your_bot` in `https://t.me/your_bot?start=promo`.
- **Text:** edit the `.eyebrow`, `.title` and `.desc` elements in `index.html`.
- **Photo:** put the original in `source-images/`, then regenerate the WebP files:

```sh
cwebp -q 80 -resize 1200 0 -metadata none source-images/hero-original.jpg -o assets/img/hero-1200.webp
cwebp -q 78 -resize 800 0 -metadata none source-images/hero-original.jpg -o assets/img/hero-800.webp
```

If the new photo has a different aspect ratio, update the `width`/`height` attributes on the `<img>`.

## Development

```sh
npm install
npm run dev      # dev server, also reachable from your phone on the same Wi-Fi
npm run build    # production build into dist/
npm run preview  # serve the dist/ build
```

Deploy the contents of `dist/`.
