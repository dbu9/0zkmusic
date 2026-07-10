Implement a complete Hugo website for my YouTube music channel, **0zkmusic**, using the **Blowfish** theme.

Do not merely describe the implementation. Create and modify all required files, run the site locally, verify the build, and leave the repository in a working state.

# 1. Project goal

Build a dark, modern music-publication website for the 0zkmusic YouTube channel.

The site will contain:

* Full music videos embedded from YouTube
* YouTube Shorts
* Downloadable MP3 files
* Cover artwork
* Lyrics
* A short story or idea behind each track
* Production notes
* Genre, mood, BPM, language, and release metadata
* Related tracks
* A conventional article/blog section for longer ideas
* About and contact pages

The site must feel like an electronic music artist or independent music label, not like a generic personal blog.

The visual direction is:

* Dark
* Minimal
* Futuristic
* Serious
* Cyber-influenced
* Clean rather than stereotypically “hacker”
* Strong typography
* Large cover artwork
* Responsive on desktop and mobile

Do not use bright gradients, excessive animation, neon overload, or a generic corporate SaaS appearance.

# 2. Technical requirements

Use:

* Hugo Extended, version 0.158.0 or newer
* Blowfish as a Hugo Module if practical
* TOML configuration files under `config/_default/`
* Hugo page bundles for tracks and articles
* Native Hugo templates, partials, and shortcodes
* Minimal custom JavaScript
* Custom CSS only where Blowfish configuration is insufficient

Do not edit files inside the Blowfish theme or Hugo module cache.

All overrides and customizations must live in the project itself, primarily under:

* `layouts/`
* `assets/`
* `static/`
* `content/`
* `archetypes/`
* `config/_default/`

The final site must build successfully with:

```bash
hugo --minify
```

The development server must work with:

```bash
hugo server --disableFastRender
```

# 3. Repository handling

First inspect the repository.

If it is empty, initialize a new Hugo site in the current repository.

If it already contains a Hugo site:

* Preserve useful existing content.
* Do not delete unrelated files without a clear reason.
* Adapt the existing structure to these requirements.
* Record significant structural decisions in the README.

Initialize Git if necessary.

Add an appropriate `.gitignore` covering at least:

```text
public/
resources/_gen/
.hugo_build.lock
.DS_Store
```

Do not commit generated `public/` output.

# 4. Install and configure Blowfish

Install Blowfish using Hugo Modules unless the existing repository already uses another valid installation method.

Expected module configuration:

```toml
[module]
  [[module.imports]]
    path = "github.com/nunocoracao/blowfish/v2"
```

Run the appropriate module initialization and dependency commands.

Create a maintainable configuration split under:

```text
config/_default/
├── hugo.toml
├── languages.en.toml
├── markup.toml
├── menus.en.toml
├── module.toml
├── params.toml
└── taxonomies.toml
```

Use the current Blowfish configuration format. Do not blindly copy obsolete configuration from old tutorials.

# 5. Site identity

Configure:

```text
Site name: 0zkmusic
Primary title: 0zkmusic
Tagline: Trance for the post-human heart.
Alternative brand phrase: We do not fight the future. We become it.
Language: English
Default content language: en
```

Use placeholder values where I have not provided the real URL:

```text
baseURL = "https://example.com/"
```

Make it obvious in the README where that value must be changed.

Create site parameters for:

* YouTube channel URL
* Contact email
* Optional Bandcamp/SoundCloud links
* Site description
* Default social preview image

Use clearly marked placeholders rather than inventing real addresses or accounts.

Suggested description:

```text
0zkmusic publishes original electronic music, trance, psytrance, melodic techno, dark techno, lyrics, visual concepts, and notes from the age after man.
```

# 6. Main information architecture

Create these primary sections:

```text
/
├── tracks/
├── articles/
├── genres/
├── moods/
├── about/
└── contact/
```

Use the navigation menu:

```text
Music
Articles
Genres
About
YouTube
```

The YouTube navigation item should be an external link and open safely in a new tab where supported.

Do not add unnecessary menu items.

# 7. Content model

Create a dedicated content type named `tracks`.

Each track must be a Hugo leaf bundle:

```text
content/tracks/track-slug/
├── index.md
├── cover.webp
└── track.mp3
```

The MP3 can be omitted when no local download exists.

Create an archetype:

```text
archetypes/tracks.md
```

The track front matter must support these fields:

```yaml
title:
date:
draft:
description:
summary:
cover:
youtube_id:
youtube_url:
youtube_short_id:
download_file:
download_enabled:
download_label:
genre:
mood:
bpm:
language:
duration:
release_date:
featured:
lyrics:
production_notes:
tags:
keywords:
```

Use arrays where multiple values are expected.

A representative front matter example:

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

mood:
  - "Aggressive"
  - "Focused"
  - "Dark"

bpm: 150
language: "English"
duration: "3:42"
release_date: 2026-07-10
featured: true

tags:
  - "AI music"
  - "trance"
  - "electronic music"

keywords:
  - "0zkmusic"
  - "psytrance"
  - "fast trance"
---

## The idea

Write the story or central idea of the track here.

## Lyrics

Write the lyrics here.

## Production notes

Write production notes here.
```

Do not render empty metadata rows when a field has no value.

# 8. Taxonomies

Configure these taxonomies:

```toml
genre = "genres"
mood = "moods"
tag = "tags"
```

Track pages must link genre, mood, and tag values to their taxonomy pages.

Create useful taxonomy list pages that show track artwork, title, summary, and metadata.

# 9. Homepage

Build a custom homepage appropriate for a music channel.

The homepage should contain, in this order:

1. Hero area
2. Featured release
3. Latest tracks
4. Genre browsing
5. Latest articles
6. YouTube call to action

## Hero

Include:

```text
0zkmusic
Trance for the post-human heart.
```

Use a restrained secondary sentence based on:

```text
Original electronic music, lyrics, visual concepts, and signals from the age after man.
```

Add two buttons:

```text
Explore Music
Watch on YouTube
```

Do not use a personal portrait.

Use either:

* A dark abstract background image, or
* A tasteful CSS background

Keep text readable and do not make the hero excessively tall.

## Featured release

Automatically select the newest non-draft track where:

```yaml
featured: true
```

Show:

* Large cover image
* Track title
* Summary
* Genre
* BPM when available
* Release date
* “Listen” button
* “Download MP3” button only when a valid download is enabled

If no featured track exists, hide this section gracefully.

## Latest tracks

Show the newest six tracks in a responsive card grid.

Each card should show:

* Cover image
* Title
* Primary genre
* Short summary
* Release date
* BPM when available

Cards must link to their track pages.

## Genres

Show compact links or cards for available genres.

Do not hard-code empty genres. Generate them from Hugo taxonomies.

## Latest articles

Show the latest three conventional articles.

## YouTube CTA

End with a clear but restrained invitation to visit the 0zkmusic YouTube channel.

# 10. Track list page

Create a custom `/tracks/` list layout.

Requirements:

* Responsive artwork-based card grid
* Newest releases first
* Cover image with consistent aspect ratio
* Title
* Genre
* Mood where space permits
* BPM
* Release date
* Short summary
* Featured indicator only when visually subtle

Do not present track releases as plain text blog entries.

Pagination is optional initially, but the implementation must not prevent adding it later.

# 11. Individual track page

Create a custom single-track layout.

The order should be:

1. Cover artwork and track heading
2. Track metadata
3. Primary YouTube embed
4. Action buttons
5. Main written content
6. Optional YouTube Short
7. Related tracks
8. Previous and next track navigation

## Header

Display:

* Cover
* Title
* Summary or description
* Genre
* Mood
* BPM
* Duration
* Language
* Release date

The design must remain clean when some fields are absent.

## YouTube player

Render the main YouTube video responsively using `youtube_id`.

Prefer Hugo’s supported YouTube shortcode behavior or a local shortcode/partial with equivalent responsive behavior.

Use privacy-enhanced YouTube embedding through:

```text
https://www.youtube-nocookie.com/
```

when implementing a custom embed.

The player must:

* Preserve 16:9 aspect ratio
* Work on mobile
* Have a useful title attribute
* Use lazy loading where possible
* Not overflow its container

If `youtube_id` is absent, omit the player.

## Shorts

Support one optional YouTube Short through `youtube_short_id`.

Render it in a portrait-oriented container suitable for a 9:16 video.

Do not stretch a Short into a 16:9 box.

Place the Short after the written sections so it does not dominate the page.

## Download button

When:

```yaml
download_enabled: true
```

and the page bundle contains the file named in `download_file`, show a prominent:

```text
Download MP3
```

button.

The button must reference the page resource correctly and continue working when the site is hosted under a subpath.

Use the HTML `download` attribute where appropriate.

Do not render a broken download button if the resource does not exist.

Display a small file note such as:

```text
MP3 · Personal listening
```

Make that wording easy to change.

Do not implement access control, payment, or DRM.

## Written content

The Markdown body should naturally support:

* The idea behind the track
* Lyrics
* Production notes
* Credits
* Additional artwork

Use Blowfish typography but tune heading spacing for music release pages.

## Related tracks

Show up to three related tracks.

Determine relationships primarily by shared genre and secondarily by shared tags or mood.

Exclude the current track.

Display artwork and title.

Hide the section if there are no suitable related tracks.

## Previous and next navigation

Provide chronological previous/next release navigation at the bottom.

# 12. Reusable components

Create reusable partials or shortcodes instead of duplicating markup.

At minimum, implement components equivalent to:

```text
layouts/partials/track-card.html
layouts/partials/track-meta.html
layouts/partials/youtube-privacy.html
layouts/partials/youtube-short.html
layouts/partials/download-button.html
layouts/partials/related-tracks.html
```

The exact filenames may differ if a cleaner Hugo structure is preferable.

Keep component responsibilities clear.

# 13. Article section

Create a normal article/blog content section at:

```text
content/articles/
```

Articles will contain longer ideas connected to music, AI, technology, art, and the future.

Article pages may use Blowfish’s standard article presentation with only minor customization.

Create one example article such as:

```text
Why Trance Still Sounds Like the Future
```

Do not fill it with large amounts of generic AI prose. Use a short, clearly identified sample text.

# 14. About page

Create an About page with restrained placeholder copy based on:

```text
0zkmusic is an independent electronic music project exploring trance, psytrance, melodic techno, dark electronic music, artificial intelligence, identity, and the boundary between the human and the post-human.
```

Include the phrase:

```text
We do not fight the future. We become it.
```

Do not invent a personal biography.

# 15. Contact page

Create a simple contact page containing:

* Placeholder email
* YouTube channel link
* Optional social links driven by configuration

Do not add a nonfunctional contact form.

# 16. Visual customization

Create custom CSS under the site’s assets directory using Blowfish’s supported override mechanism.

Visual requirements:

* Dark-first design
* Neutral near-black background
* High readability
* Restrained accent color
* Strong but not oversized headings
* Rounded corners used sparingly
* Cover cards with consistent dimensions
* Subtle hover behavior
* Clear focus states
* Good contrast
* No continuous animations
* No parallax
* No visualizer effects
* No autoplaying audio or video
* No background video

Use CSS variables where practical.

Do not hard-code every style inline.

Do not add a large external CSS framework.

Respect:

```css
@media (prefers-reduced-motion: reduce)
```

# 17. Images

Use Hugo page resources for cover images.

Generate responsive image markup where practical.

Avoid cumulative layout shift by defining dimensions or aspect ratios.

Use WebP for sample artwork.

Since no final branded artwork is supplied, create simple local placeholder cover images or clearly named placeholder files. Do not hotlink random internet images.

Every meaningful image must have useful alt text.

# 18. Audio and download behavior

Do not add an automatic audio player unless it can be done cleanly and accessibly.

The primary listening action is YouTube.

The MP3 is a direct optional download.

Ensure common static-host deployment does not cause the MP3 to be omitted from the generated site.

Verify the generated output contains the sample MP3 resource or a clearly marked placeholder test resource.

If creating a dummy test MP3 is inappropriate, create a small placeholder text file for structural testing and document where the real MP3 must be placed. Do not mislabel a text file as MP3.

# 19. SEO and social sharing

Configure:

* Site description
* Canonical URLs
* Open Graph metadata
* Twitter/X card metadata where supported
* Sitemap
* RSS
* Robots file
* Per-track description
* Cover image as the social image for track pages
* Meaningful page titles

Track page title format should be approximately:

```text
Track Title | 0zkmusic
```

Add JSON-LD structured data for individual tracks using a suitable Schema.org type, preferably `MusicRecording`.

Include when available:

* Name
* Description
* Image
* Date published
* Duration
* Genre
* URL
* YouTube embed URL
* Download URL

Do not emit invalid fields when values are absent.

# 20. Privacy and performance

Use privacy-enhanced YouTube embeds where possible.

Do not load YouTube iframe resources on pages without videos.

Prefer thumbnail or lazy-loading behavior if it can be implemented reliably without excessive JavaScript.

Do not add:

* Google Analytics
* Advertising trackers
* Facebook Pixel
* Third-party chat widgets
* Unnecessary external fonts

Use local/system fonts unless Blowfish already handles typography without an additional tracking concern.

Document that a YouTube embed still contacts YouTube when activated or loaded.

# 21. Accessibility

Ensure:

* Keyboard-visible focus states
* Logical heading hierarchy
* Alt text
* Buttons and links have descriptive labels
* Embedded videos have titles
* Sufficient color contrast
* No information conveyed solely by color
* Responsive layout at approximately 320px width
* No horizontal page scrolling
* Download links clearly identify their action

# 22. Sample content

Create at least three sample tracks representing the intended site:

1. `pulse-in-my-veins`
2. `cotard`
3. `fly-this-night`

Use clearly marked placeholder YouTube IDs so the site does not accidentally embed unrelated videos.

Give each track:

* Distinct genre and mood values
* Cover placeholder
* Summary
* Short idea section
* Short lyrics placeholder
* Production notes placeholder
* BPM
* Language
* Release date

Enable the download button only for a sample where an actual test resource exists.

At least one track should be featured.

Also create:

* One sample article
* About page
* Contact page
* Section index files

# 23. Content creation workflow

Make adding a track straightforward.

The expected author workflow should be:

```bash
hugo new content tracks/new-track/index.md
```

After creation, the author should only need to:

1. Edit front matter
2. Add `cover.webp`
3. Optionally add `track.mp3`
4. Add the YouTube video ID
5. Write Markdown content
6. Set `draft: false`

Ensure the archetype supports this workflow.

# 24. Documentation

Create or update `README.md`.

Document:

* Requirements
* Hugo version requirement
* How Blowfish is installed
* Local development command
* Production build command
* Directory structure
* How to create a track
* How to add cover artwork
* How to embed a full YouTube video
* How to embed a YouTube Short
* How to enable or disable MP3 downloads
* How to add an article
* How to change site URL
* How to change YouTube channel URL
* How to customize site colors
* Where custom templates live
* How to update Blowfish safely
* Deployment notes for a generic static host
* Privacy implications of YouTube embeds

Include a complete sample track front matter block in the README.

# 25. Validation

Before finishing, run all relevant checks.

At minimum:

```bash
hugo version
hugo mod get -u
hugo mod tidy
hugo --gc --minify
```

Also inspect the generated site for:

* Broken internal links
* Missing resources
* Missing covers
* Broken download paths
* Template errors
* Invalid taxonomy links
* Empty components
* Mobile overflow

Run the development server if the environment permits and inspect the important pages.

Verify:

```text
/
 /tracks/
 /tracks/pulse-in-my-veins/
 /articles/
 /about/
 /contact/
 /genres/
 /moods/
```

The final `hugo --gc --minify` command must exit successfully.

# 26. Completion report

At completion, provide a concise report containing:

1. What was implemented
2. Important files created or modified
3. Commands used to run and build the site
4. Any placeholders I still need to replace
5. Any limitations or decisions that deserve attention
6. Confirmation that the production build succeeds

Do not claim that something works unless you actually verified it.

# 27. Implementation priorities

Use this order of priority:

1. Correct Hugo build
2. Maintainable content model
3. Excellent track pages
4. Artwork-focused homepage and track grid
5. Responsive design
6. MP3 download correctness
7. YouTube and Shorts embeds
8. SEO and structured metadata
9. Extra visual polish

Avoid overengineering. The implementation should be clean, understandable, and easy to extend.

