# Print Room — Text-to-Image Chat Bot

A single self-contained web app (`index.html`) that works like a chat bot:
you type a description, and it "develops" a high-quality AI-generated image
in the chat thread — like a photo appearing in a darkroom tray.

## How to run it

No install, no server, no build step.

1. Unzip this folder.
2. Double-click `index.html` (or right-click → Open with → your browser).
3. Type a prompt and hit **Enter** (or click **Develop**).

That's it. It runs entirely in your browser.

> Requires an internet connection — image generation happens through a free
> public API (see below), not on your machine.

## How the image generation works

The app calls **Pollinations.ai** (`https://image.pollinations.ai`), a free
image-generation service that needs no API key and no sign-up. It uses the
`flux` model at large pixel dimensions for high-quality output, plus a
prompt-enhancement pass, and appends quality-boosting keywords
(`ultra high quality, highly detailed, 8k, masterpiece`) when the
**High-quality upscale pass** toggle is on.

Every request is built in `buildImageURL()` inside the `<script>` tag near
the bottom of `index.html`:

```js
function buildImageURL(prompt, seed){
  const base = 'https://image.pollinations.ai/prompt/';
  const params = new URLSearchParams({
    width: currentAR.w,
    height: currentAR.h,
    seed: seed,
    nologo: 'true',
    model: 'flux',
    enhance: enhanceToggle.checked ? 'true' : 'false'
  });
  return base + encodeURIComponent(prompt) + '?' + params.toString();
}
```

## Features

- **Chat-style interface** — prompts and generated images appear as a
  running conversation thread.
- **Frame shapes** — Square, Portrait, Landscape, Widescreen (each maps to
  specific high-resolution pixel dimensions).
- **Style presets** — Photorealistic, Cinematic, Oil painting, Anime,
  Watercolor, Isometric 3D, Analog vintage, Concept art — each appends
  tuned descriptor keywords to your prompt.
- **Quality controls** — high-quality upscale keywords, prompt enhancement,
  and an optional locked seed for reproducible variations.
- **Download & reprint** — every generated image has a download button and
  a "Reprint" button (regenerates with a new seed).
- **No backend, no dependencies** — everything runs client-side in one
  HTML file.

## Swapping in a different image model

If you'd rather use a paid/higher-control provider (OpenAI Images, Stability
AI, Google Imagen, your own Stable Diffusion server, etc.), you only need to
edit **`buildImageURL()`** and the `img.onload`/`img.onerror` handling in
`index.html`. Most providers require an API key and a server-side proxy
(since browser JavaScript can't safely hold a secret key) — in that case,
have `buildImageURL()`/the fetch call point at your own small backend
instead of calling the provider directly from the browser.

## Customizing the look

All styling is in the `<style>` block at the top of `index.html` — colors,
fonts (Fraunces / Space Grotesk / JetBrains Mono, loaded from Google Fonts),
spacing, and the "developing photo" reveal animation are all defined there
with CSS custom properties (`:root { --bg: ...; --safelight: ...; }`) so you
can re-theme it quickly.

## Files

```
index.html   The entire app (HTML + CSS + JS, single file)
README.md    This file
```
