# 0zkmusic

Dark Hugo website for the 0zkmusic YouTube music channel, built with the Blowfish theme as a Hugo Module.

## Requirements

- Hugo Extended `0.158.0` or newer. This repository was verified with Hugo Extended `0.163.3`.
- Go, required by Hugo Modules.

Blowfish is installed through `config/_default/module.toml`:

```toml
[module]
  [[module.imports]]
    path = "github.com/nunocoracao/blowfish/v2"
```

## Local Development

```bash
hugo mod tidy
hugo server --disableFastRender
```

Production build:

```bash
hugo --gc --minify
```

The placeholder site URL is configured in `config/_default/hugo.toml`:

```toml
baseURL = "https://example.com/"
```

Change this before deploying.

## Structure

- `config/_default/`: split Hugo and Blowfish configuration.
- `content/tracks/`: track leaf bundles with Markdown, cover art, and optional MP3 files.
- `content/articles/`: conventional article/blog content.
- `layouts/tracks/`: custom track list and single release layouts.
- `layouts/partials/`: reusable track cards, metadata, embeds, downloads, related tracks, and JSON-LD.
- `assets/css/custom.css`: dark 0zkmusic visual overrides loaded by Blowfish.
- `static/img/`: fallback and social preview images.

Generated output lives in `public/` and is ignored by Git.

## Creating A Track

Use the track archetype:

```bash
hugo new content tracks/new-track/index.md
```

Then:

1. Edit the front matter.
2. Add `cover.webp` to the same bundle directory.
3. Optionally add `track.mp3`.
4. Add the YouTube video ID.
5. Write the Markdown body.
6. Set `draft: false`.

Sample front matter:

```yaml
---
title: "Pulse in My Veins"
date: 2026-07-10
draft: false
description: "Fast, synth-led trance about focus, force, and forward motion."
summary: "A fast minor-key trance track driven by tension and determination."
cover: "cover.webp"

youtube_id: "REPLACE_WITH_VIDEO_ID"
youtube_url: "https://www.youtube.com/watch?v=REPLACE_WITH_VIDEO_ID"
youtube_short_id: ""

download_file: "track.mp3"
download_enabled: true
download_label: "Download MP3"

genre:
  - "Psytrance"
  - "Progressive Trance"
genres:
  - "Psytrance"
  - "Progressive Trance"

mood:
  - "Aggressive"
  - "Focused"
  - "Dark"
moods:
  - "Aggressive"
  - "Focused"
  - "Dark"

bpm: 150
language: "English"
duration: "3:42"
release_date: 2026-07-10
featured: true

lyrics: ""
production_notes: ""

tags:
  - "AI music"
  - "trance"
  - "electronic music"

keywords:
  - "0zkmusic"
  - "psytrance"
  - "fast trance"
---
```

Hugo uses the plural `genres` and `moods` keys to generate taxonomy pages. Keep them in sync with the singular `genre` and `mood` fields, which are used by the custom track templates.

## Covers

Track cover artwork should be named `cover.webp` by default and placed in the track bundle:

```text
content/tracks/track-slug/cover.webp
```

The templates use Hugo page resources and generate optimized derivatives. If no cover exists, `static/img/placeholder-cover.svg` is used.

## YouTube Embeds

For a full video, set:

```yaml
youtube_id: "VIDEO_ID"
youtube_url: "https://www.youtube.com/watch?v=VIDEO_ID"
```

For a Short, set:

```yaml
youtube_short_id: "SHORT_ID"
```

Embeds use `https://www.youtube-nocookie.com/`, preserve aspect ratio, lazy load, and include title attributes. A YouTube embed can still contact YouTube when loaded.

## MP3 Downloads

To enable a download:

```yaml
download_file: "track.mp3"
download_enabled: true
download_label: "Download MP3"
```

Place the MP3 at:

```text
content/tracks/track-slug/track.mp3
```

The download button is only rendered when `download_enabled` is true and the bundle resource exists. The sample `pulse-in-my-veins` bundle includes a short test MP3. Replace it with the real file before publishing.

The small file note is controlled by `downloadNote` in `config/_default/params.toml`.

## Articles

Create articles under `content/articles/`:

```bash
hugo new content articles/my-article/index.md
```

Articles use Blowfish's standard article presentation with the site's dark styling.

## Site Links And Placeholders

Change these placeholders before deployment:

- `baseURL` in `config/_default/hugo.toml`
- `youtubeChannelURL` in `config/_default/params.toml`
- `contactEmail` in `config/_default/params.toml`
- Menu YouTube URLs in `config/_default/menus.en.toml`
- Author links in `config/_default/languages.en.toml`
- Placeholder YouTube IDs in sample track front matter
- Sample cover art and sample MP3

Optional Bandcamp and SoundCloud URLs are also in `config/_default/params.toml`.

## Styling

Custom colors and layout rules live in `assets/css/custom.css`. The site uses a dark neutral base, restrained cyan accent, consistent square artwork, responsive grids, visible focus states, and reduced-motion handling.

Blowfish files and the Hugo module cache are not edited. Override templates live in `layouts/`.

## Updating Blowfish

Use Hugo Modules:

```bash
hugo mod get -u github.com/nunocoracao/blowfish/v2
hugo mod tidy
hugo --gc --minify
```

If Hugo reports a theme compatibility warning, use a Hugo Extended version within the range declared by the installed Blowfish release.

## Deployment

Build with:

```bash
hugo --gc --minify
```

Deploy the generated `public/` directory to a static host. Do not commit `public/`.

The site emits canonical URLs, sitemap, RSS feeds, robots.txt, Open Graph/Twitter metadata, per-track cover metadata, and MusicRecording JSON-LD for track pages.

## Privacy

No analytics, advertising trackers, chat widgets, or external font services are configured. YouTube embeds use the privacy-enhanced `youtube-nocookie.com` domain, but YouTube may still receive data when a video iframe loads or is activated.
