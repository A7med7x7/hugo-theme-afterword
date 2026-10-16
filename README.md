# Nightfall A7

A reading-focused Hugo theme for personal essays and notes. It started as a fork of [Nightfall](https://github.com/LordMathis/hugo-theme-nightfall) by Matúš Námešný. On top of Nightfall it adds a serif reading layout, a light mode, post series ("sequences"), full-text search, and Open Graph tags.

See it in use at [a7med7x7.github.io](https://a7med7x7.github.io/).

![Dark mode post](https://raw.githubusercontent.com/A7med7x7/hugo-theme-nightfall-a7/main/images/screenshot.png)

| Light mode | Sequences |
| --- | --- |
| ![Light mode post](https://raw.githubusercontent.com/A7med7x7/hugo-theme-nightfall-a7/main/images/screenshot_light.png) | ![Sequence page](https://raw.githubusercontent.com/A7med7x7/hugo-theme-nightfall-a7/main/images/screenshot_sequences.png) |

## Why it's a separate theme

Nightfall is a minimal terminal-style theme. Nightfall A7 keeps its header and color base but redesigns everything else for long-form reading, and adds features Nightfall doesn't have:

- EB Garamond typography, plus a reading progress bar on posts
- Light and dark modes that follow the reader's system setting
- Post lists shown as cards, with pagination
- Sequences, which group posts into an ordered series with a cover card, a reading checklist, and previous/next links on each post
- A search page powered by [Pagefind](https://pagefind.app/)
- Open Graph and Twitter card tags, with optional per-post images
- A reworked header, footer, and social links

## Requirements

- Hugo **extended** 0.128.0 or newer
- [Dart Sass](https://gohugo.io/functions/css/sass/#dart-sass) on your `PATH`
- Node.js, but only if you want search (to run Pagefind)

## Install

As a Git submodule:

```bash
git submodule add https://github.com/A7med7x7/hugo-theme-nightfall-a7.git themes/hugo-theme-nightfall-a7
```

Then set the theme in `hugo.toml`:

```toml
theme = "hugo-theme-nightfall-a7"
```

Or as a Hugo module (needs Go):

```bash
hugo mod init github.com/you/your-site
```

```toml
[module]
  [[module.imports]]
    path = "github.com/A7med7x7/hugo-theme-nightfall-a7"
```

To try it with the example content:

```bash
git clone https://github.com/A7med7x7/hugo-theme-nightfall-a7.git
cd hugo-theme-nightfall-a7/exampleSite
hugo server --themesDir ../..
```

## Configuration

[exampleSite/hugo.toml](exampleSite/hugo.toml) is a complete working config. The main settings:

```toml
title = "My Blog"

[params]
user = "me"                 # the header shows me@example.org ~ $
hostname = "example.org"
siteDescription = "Used for meta and Open Graph descriptions."
openLinksInNewTab = true    # external links open in a new tab

[params.author]
name = "Your Name"          # home page heading, page titles, footer
email = "you@example.org"
# avatar = "/avatar.png"
# avatarSize = "size-m"     # size-xs, size-s, size-m, size-l, size-xl
# avatarFirst = false       # show the avatar above your name
```

Posts go in `content/posts/`.

### Menu

```toml
[menu]
  [[menu.header]]
    name = "posts"
    weight = 10
    url = "/posts"
  [[menu.header]]
    name = "sequences"
    weight = 15
    url = "/sequences"
  [[menu.header]]
    name = "search"
    weight = 20
    url = "/search"
```

### Social links

Icons come from [Font Awesome 6](https://fontawesome.com/icons).

```toml
[[params.social]]
key = 0
name = "github"
url = "https://github.com/you"
icon = "fa-brands fa-github"
target = "_blank"
aria = "GitHub"
```

### Fonts

The theme loads EB Garamond from Google Fonts. To use another font, set the stylesheet URL and the CSS font stacks:

```toml
[params.fonts]
url = "https://fonts.googleapis.com/css2?family=Lora:ital,wght@0,400;0,600;1,400&display=swap"
body = "'Lora', serif"
heading = "'Lora', serif"   # defaults to the body font
```

### Sequences

A sequence is an ordered series of posts. First, turn on the `series` taxonomy:

```toml
[taxonomies]
tag = "tags"
category = "categories"
series = "series"
```

Create `content/sequences/_index.md` for the index page. Its body text becomes the intro:

```toml
+++
title = "Sequences"
intro_label = "Series"                    # optional small label
intro_title = "Posts, gathered in order"  # optional, defaults to the title
+++
Pick a sequence to start reading.
```

Add one folder per sequence, such as `content/sequences/getting-started/_index.md`:

```toml
+++
title = "Getting Started"
description = "Shown on the sequence card."
sequence_key = "getting-started"
layout = "sequence-single"
cover_emoji = "🌙"
cover_color = "#3b4a5a"
+++
```

Then give each post in the sequence the same key and its position:

```toml
series = ["getting-started"]
series_order = 1
```

### Search

Add a search page at `content/search.md`:

```yaml
---
title: "Search"
layout: "search"
---
```

The page reads a Pagefind index, so build that index after Hugo runs:

```bash
hugo
npx pagefind --site public
```

> [!IMPORTANT]
> `hugo server` doesn't run Pagefind, so search returns nothing during local development. To test it, run the two commands above and serve `public/`.

### Open Graph images

Every page gets Open Graph and Twitter tags. The theme picks the image in this order:

1. The first entry of `images` in the page's front matter
2. If `ogImagesDir` is set: `<ogImagesDir>/<post-file-name>.png` for posts, `<ogImagesDir>/_site.png` for every other page
3. `ogImage`, one image for the whole site

```toml
[params]
ogImage = "/images/og-default.png"
# ogImagesDir = "/og"
```

### Favicon

```toml
[params]
favicon = "/favicon.png"   # defaults to the theme's /logo/favicon.svg
```

### Custom head and footer

To add tags to `<head>`, such as analytics, create `layouts/partials/custom-head.html` in your site.

To replace the footer text, use `footerHtml`:

```toml
[params]
footerHtml = '© 2026 Your Name'
```

### Post front matter

```toml
title = "My post"
date = 2026-01-01
description = "Shown under the title and in previews."
tags = ["notes"]
feature_image = "/images/cover.jpg"   # optional, shown above the post
```

## Credits

Based on [Nightfall](https://github.com/LordMathis/hugo-theme-nightfall) by [Matúš Námešný](https://namesny.com), released under the MIT license. The example content comes from Nightfall and the Hugo docs.

## License

[MIT](LICENSE)
