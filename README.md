# 0zkMusic

Dark Hugo website for the 0zkMusic YouTube music channel, built with the Blowfish theme as a Hugo Module.

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

Site URL in `config/_default/hugo.toml`:

```toml
baseURL = "https://www.0zkmusic.com/"
```

## Structure

- `config/_default/`: split Hugo and Blowfish configuration.
- `content/tracks/`: track leaf bundles with Markdown, cover art, and optional MP3 files.
- `content/articles/`: conventional article/blog content.
- `layouts/tracks/`: custom track list and single release layouts.
- `layouts/partials/`: reusable track cards, metadata, embeds, downloads, related tracks, JSON-LD, and branded favicon markup.
- `assets/css/custom.css`: dark 0zkMusic visual overrides loaded by Blowfish.
- `static/img/`: fallback and social preview images.
- `static/img/0zkmusic-profile.jpg`: YouTube channel profile image used as the site logo and homepage brand mark.
- `static/img/0zkmusic-banner.jpeg`: Twitter/X banner image used as the homepage hero background and default social image.
- `static/favicon-*`, `static/favicon.ico`, and platform icon files: favicon set generated from the 0zkMusic profile image.

Generated output lives in `public/` and is ignored by Git.

## Creating A Track

Use the track archetype:

```bash
hugo new content tracks/new-track/index.md
```

Then:

1. Edit the front matter.
2. Add `cover.webp` to the same bundle directory.
3. Optionally add `lyrics.txt` and `track.mp3`.
4. Add the YouTube video ID.
5. Write the Markdown body for the idea, production notes, and other non-lyric sections.
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
lyrics_file: "lyrics.txt"
production_notes: ""
production_tool: ""
production_model: ""
production_custom_model: ""
production_custom_model_url: ""

tags:
  - "AI music"
  - "trance"
  - "electronic music"

keywords:
  - "0zkMusic"
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

## Lyrics

For new track pages, store lyrics in the track bundle as:

```text
content/tracks/track-slug/lyrics.txt
```

Set this front matter field:

```yaml
lyrics_file: "lyrics.txt"
```

The track template renders `lyrics.txt` as Markdown in the Listen section next to the YouTube embed. Keep `index.md` for the idea and other interpretive, non-lyric sections.

## Production Metadata

Production notes are rendered as a compact block below the idea section, not as a large article section. Use short values such as:

```yaml
production_tool: "Suno"
production_model: "5.5"
production_custom_model: "AN"
production_custom_model_url: "/articles/model-an/"
production_notes: "Made with Suno 5.5 using the custom AN model."
```

Use `production_custom_model_url` to link to a site article that explains how a pretrained/custom model was created.

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

Local file in the track bundle:

```yaml
download_file: "track.mp3"
download_url: ""
download_enabled: true
download_label: "Download MP3"
```

```text
content/tracks/track-slug/track.mp3
```

External host (e.g. Proton Drive):

```yaml
download_file: ""
download_url: "https://drive.proton.me/urls/..."
download_enabled: true
download_label: "Download MP3"
```

The button shows when `download_enabled` is true and either `download_url` is set or the local `download_file` resource exists. External links open in a new tab. Structured data advertises MP3 encoding only for local bundle files, because share pages such as Proton Drive are HTML pages rather than direct `audio/mpeg` resources.

## Articles

Create articles under `content/articles/`:

```bash
hugo new content articles/my-article/index.md
```

Articles use Blowfish's standard article presentation with the site's dark styling.

## Site Links And Placeholders

The YouTube channel URL is configured as `https://www.youtube.com/@0zkMusic`.

Optional remaining config:

- Bandcamp / SoundCloud URLs in `config/_default/params.toml`
- Google Analytics (GA4) Measurement ID via `[services.googleAnalytics]` in `config/_default/hugo.toml`

## Styling

Custom colors and layout rules live in `assets/css/custom.css`. The site uses the existing 0zkMusic image palette: violet-black base, magenta primary accent, and cyan-green signal highlights. It keeps consistent square artwork, responsive grids, visible focus states, and reduced-motion handling.

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

Netlify is configured via `netlify.toml` (Hugo Extended build, publish `public/`). Production builds set `HUGO_ENV=production` so Google Analytics loads.

Local / manual build:

```bash
hugo --gc --minify
```

Do not commit `public/`.

The site emits canonical URLs, sitemap, RSS feeds, robots.txt, Open Graph/Twitter metadata, per-track cover metadata, and MusicRecording JSON-LD for track pages.

## Privacy

Google Analytics 4 is enabled in production builds only (`hugo.IsProduction`), via Blowfish’s built-in gtag integration and `[services.googleAnalytics]` in `config/_default/hugo.toml`. Local `hugo server` does not inject the tag.

No advertising trackers, chat widgets, or external font services are configured. YouTube embeds use the privacy-enhanced `youtube-nocookie.com` domain, but YouTube may still receive data when a video iframe loads or is activated.
