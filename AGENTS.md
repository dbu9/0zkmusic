# AGENTS.md

Guidance for coding agents working in this repository.

## Project Overview

This is a Hugo Extended site for the 0zkMusic YouTube music channel, built with the Blowfish theme as a Hugo Module.

The site is a dark electronic-music publication site. Keep the tone and UI aligned with the existing brand: violet-black base, magenta accents, cyan-green signal highlights, large cover art, and a serious futuristic music-label feel.

## Important Commands

Use Hugo Extended 0.158.0 or newer. The repository has been verified with Hugo Extended 0.163.3.

```bash
hugo mod tidy
hugo server --disableFastRender
hugo --gc --minify
```

If a local `hugo server` is already running, do not start another one on the same port. Check first with:

```bash
pgrep -af "hugo server"
```

For a clean validation that is not affected by live-reload files from a running dev server, build to a temporary destination:

```bash
rm -rf /tmp/0zkmusic-public-check
hugo --gc --minify --destination /tmp/0zkmusic-public-check
```

## Repository Rules

- Do not edit files inside the Blowfish theme or Hugo module cache.
- Put project overrides in `layouts/`, `assets/`, `static/`, `content/`, `archetypes/`, or `config/_default/`.
- Do not commit generated output.
- `public/`, `resources/_gen/`, `.hugo_build.lock`, and `.DS_Store` must remain ignored.
- Preserve user changes in the working tree. If unrelated files are dirty, leave them alone.
- Keep `README.md` accurate when changing site structure, commands, or deployment assumptions.

## Content Structure

Track pages are Hugo leaf bundles:

```text
content/tracks/track-slug/
├── index.md
├── lyrics.txt
├── cover.webp
└── track.mp3
```

The lyrics and MP3 files are optional. Use `lyrics.txt` for lyrics when lyrics exist; the template renders it as Markdown in the Listen section next to the YouTube embed. Set `download_enabled: true` only when either `download_url` points to an intended external download page or the matching `download_file` bundle resource exists.

Track front matter should keep these paired taxonomies in sync:

```yaml
genre:
genres:
mood:
moods:
```

The custom templates use the singular keys, and Hugo taxonomy pages use the plural keys.

Use `cover.webp` for track art. If a YouTube thumbnail is used as a cover, generate a local `cover.webp` in the track bundle rather than hotlinking the thumbnail.

Keep the main track body for the idea and other interpretive, non-lyric sections. Do not put a `## Lyrics` section in `index.md` for new pages; put lyrics in `lyrics.txt` and set:

```yaml
lyrics_file: "lyrics.txt"
```

Do not put `## Production notes` in `index.md`. Production details render as a compact metadata block from front matter:

```yaml
production_tool: "Suno"
production_model: "5.5"
production_custom_model: "AN"
production_custom_model_url: "/articles/model-an/"
production_notes: "Made with Suno 5.5 using the custom AN model."
```

## YouTube Content Updates

The public channel handle is:

```text
https://www.youtube.com/@0zkMusic
```

The channel ID is:

```text
UC1-tLYf1HiKL5v0kHY90IvQ
```

The public feed is useful for recent videos, titles, thumbnails, publish dates, hashtags, and descriptions:

```text
https://www.youtube.com/feeds/videos.xml?channel_id=UC1-tLYf1HiKL5v0kHY90IvQ
```

When importing videos:

- Skip videos that already have a track page with the same `youtube_id`.
- If an existing placeholder page clearly matches a real channel video, update the existing page instead of duplicating it.
- Read lyrics from the YouTube description when present.
- Preserve original lyrics and translations from the description.
- Write lyrics to `lyrics.txt`, not to a `## Lyrics` section in `index.md`.
- Write production details to front matter, not to a `## Production notes` section in `index.md`.
- Use hashtags from the description/title to populate genre and tag metadata.
- Use the public watch URL in `youtube_url`.
- Use `youtube_short_id` for teaser/short videos that belong to an existing full track.
- Do not invent lyrics when the description does not include them.

## Styling And Templates

- Custom styling belongs in `assets/css/custom.css`.
- Template overrides belong under `layouts/`.
- Keep YouTube embeds on `youtube-nocookie.com` via the existing partials.
- Keep the site responsive on mobile and desktop.
- Avoid bright gradients, excessive animation, neon overload, and generic SaaS styling.
- Use the existing profile and banner assets when extending brand-heavy sections:
  - `static/img/0zkmusic-profile.jpg`
  - `static/img/0zkmusic-banner.jpeg`

## Validation Checklist

Before finishing meaningful changes, run:

```bash
hugo --gc --minify
```

For content or URL-heavy changes, also run a clean temp build and check local links. A simple Python HTML parser link scan is enough; ignore external `http` and `https` targets unless the task is specifically about external links.

Report any commands that could not be run.
