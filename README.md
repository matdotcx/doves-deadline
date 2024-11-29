# Doves Theme for Hugo

Created in the Cotswolds in 2024, this typography-focused Hugo theme draws inspiration from one of type history's most dramatic stories. In 1900, at Hammersmith's Doves Press, a unique typeface was commissioned by T.J. Cobden-Sanderson and Emery Walker. Crafted by master punchcutter Edward Prince and based on Nicolas Jenson's pioneering 15th-century Venetian type, it would become legendary not just for its beauty, but for its fate. Between 1913 and 1917, in a series of nighttime visits to Hammersmith Bridge, Cobden-Sanderson threw every piece of the type into the Thames – an act of typographic sacrifice choosing destruction over compromise.

Nearly a century later, Robert Green began an extraordinary revival project. After three years of initial research and drawing, in 2014 he worked with the Port of London Authority's diving team to recover original metal sorts from the riverbed. Those recovered pieces, along with extensive archival research, informed his meticulous digital reconstruction, culminating in the 2022 revision used in this theme.

We employ both variants of Green's latest work: Doves Type Text at 20px for body content, preserving the soft letterpress-like quality of the original while adapting to modern digital needs, and Doves Type Headline at 21.25px for titles. Each paragraph opens with a drop cap in our signature orange (`#FF7900`), a personal choice reflecting both our fondness for the color and this theme's human-centered approach to technology.

Like the metal sorts recovered piece by piece from the Thames through patience, technology, and human determination, this theme attempts to bridge gaps between craft and tools, between timeless principles and modern needs.

## Table of Contents

- [Features](#features)
- [Requirements](#requirements)
  - [Hugo Version](#hugo-version)
  - [Required Fonts](#required-fonts)
- [Quick Start](#quick-start)
- [Installation](#installation)
- [Content Features](#content-features)
  - [Build Statistics Integration](#build-statistics-integration)
  - [Edition Counter](#edition-counter)
  - [Build Statistics](#build-statistics)
  - [Blockquotes](#blockquotes)
- [Customization](#customization)
  - [Colors](#colors)
  - [Dark Mode](#dark-mode)
- [Technical Details](#technical-details)
  - [Directory Structure](#directory-structure)
  - [Performance Considerations](#performance-considerations)
  - [Browser Support](#browser-support)
- [Troubleshooting](#troubleshooting)
  - [Font Installation Issues](#font-installation-issues)
  - [Build Statistics Not Appearing](#build-statistics-not-appearing)
  - [Drop Cap Rendering](#drop-cap-rendering)
- [Credits & License](#credits--license)
  - [Fonts](#fonts)
  - [Colors](#colors-1)
  - [License](#license)
  - [Author](#author)

---
## Features

A typography-focused Hugo theme inspired by the historic Doves Type. This theme emphasizes readability, whitespace, and elegant type presentation, featuring:

- Typography-first design approach
- Responsive layout
- Dark mode with color-matched palettes
- Drop caps for article openings
- Edition counter for content versioning
- Build statistics integration
- Syntax highlighting with Flexoki colors
- RSS feed support
- Blockquote styling with accent colors
- Clean archive page layout

---
## Requirements

### Hugo Version
Requires Hugo Extended v0.137.1 or later.

### Required Fonts
This theme is designed to use Doves Type, a commercial digital revival:

- Doves Type Headline (21.25px for titles)
- Doves Type Text (20px for body content)

Font files needed in your `/static/type/` directory:
- DovesTypeHeadline-Regular.woff2
- DovesTypeHeadline-Regular.woff
- DovesTypeText-Regular.woff2
- DovesTypeText-Regular.woff

For code blocks, the theme uses Fira Code (included):
- FiraCode-Light.woff2
- FiraCode-Bold.woff2
- FiraCode-Medium.woff2
- FiraCode-Regular.woff2

---
## Quick Start

1. Install Hugo Extended v0.137.1 or later
2. Create a new site and add the theme:
```bash
hugo new site yoursite
cd yoursite
git submodule add https://github.com/yourusername/hugo-doves-theme.git themes/doves
```
3. Copy the example configuration:
```bash
cp themes/doves/exampleSite/hugo.toml .
```
4. Start the development server:
```bash
hugo server -D
```

---
## Installation

1. Create a new Hugo site:
```bash {title="Create New Site"}
hugo new site yoursite
cd yoursite
```

2. Add the theme:
```bash {title="Add Theme"}
git submodule add https://github.com/yourusername/hugo-doves-theme.git themes/doves
```

3. Update your hugo.toml:
```toml {title="Configuration"}
baseURL = "https://example.org/"
languageCode = "en-us"
title = "Your Site Title"
theme = "doves"
enableGitInfo = true

[markup]
  [markup.goldmark]
    [markup.goldmark.renderer]
      unsafe = true
  [markup.highlight]
    codeFences = true
    guessSyntax = false
    lineNoStart = 1
    lineNos = true
    lineNumbersInTable = true
    noClasses = false
    tabWidth = 2

[params]
  description = "Your site description"
  footer = "Your footer text"

[security]
  [security.exec]
    allow = ["^git$"]
    osEnv = ["(?i)^((HTTPS?|NO)_PROXY|PATH(EXT)?|APPDATA|HOME|TEMP|TMP|USERPROFILE)$"]
```

---
## Content Features

### Build Statistics Integration
Add this to your GitHub Actions workflow:

```yaml {title="GitHub Actions Workflow"}
- name: Generate Build Statistics
  run: |
    mkdir -p data/stats
    # Get total commit count for the repository
    TOTAL_COMMITS=$(git rev-list --count HEAD)
    echo "TOTAL_COMMITS=${TOTAL_COMMITS}" >> $GITHUB_ENV

    # Calculate time since first commit
    FIRST_COMMIT_DATE=$(git log --reverse --format=%at | head -1)
    DAYS_SINCE_START=$(( ($(date +%s) - $FIRST_COMMIT_DATE) / 86400 ))
    echo "DAYS_SINCE_START=${DAYS_SINCE_START}" >> $GITHUB_ENV

    # Get commit counts per directory
    CONTENT_COMMITS=$(git rev-list --count HEAD -- content/ || echo "0")
    THEME_COMMITS=$(git rev-list --count HEAD -- themes/ || echo "0")

    # Generate statistics JSON
    cat << EOF > data/stats/build_info.json
    {
      "hugo_version": "${HUGO_VERSION}",
      "total_commits": ${TOTAL_COMMITS},
      "days_since_start": ${DAYS_SINCE_START},
      "content_commits": ${CONTENT_COMMITS},
      "theme_commits": ${THEME_COMMITS},
      "build_date": "$(date -u +"%Y-%m-%dT%H:%M:%SZ")",
      "build_number": "${GITHUB_RUN_NUMBER}"
    }
    EOF
```

### Edition Counter
Posts automatically track their revision history using Git data.

### Build Statistics
Use the stats shortcode to display build information:
```go {title="Stats Shortcode"}
{{</* stats */>}}
```

### Blockquotes
Styled blockquotes with left border accent:
```md {title="Blockquote Example"}
<blockquote>
Your quote text here
</blockquote>
```

---
## Customization

### Colors
Theme colors are defined in CSS variables in assets/css/style.css:

```css {title="Color Variables"}
:root {
    --background-color: rgb(248, 245, 240);
    --text-color: #232323;
    --link-color: #ff7900;
    --dropcap-color: #ff7900;
    /* ... other variables ... */
}
```

### Dark Mode
Dark mode colors are customizable via the [data-theme="dark"] selector.

---
## Technical Details

### Directory Structure
```md {title="Theme Structure"}
themes/doves/
├── archetypes/
│   └── default.md
├── assets/
│   └── css/
│       ├── style.css
│       └── syntax.css
├── layouts/
│   ├── _default/
│   │   ├── archives.html
│   │   ├── baseof.html
│   │   ├── list.html
│   │   └── single.html
│   ├── partials/
│   │   ├── build-stats.html
│   │   ├── edition-counter.html
│   │   ├── footer.html
│   │   ├── head.html
│   │   ├── header.html
│   │   └── ordinal.html
│   └── shortcodes/
│       └── stats.html
├── static/
│   └── type/  # Type directory (typefaces not included)
├── LICENSE
├── README.md
├── theme.toml
└── .gitignore
```

### Performance Considerations
- Font files use `font-display: block`
- CSS is automatically minified in production
- Images should be placed in `/static/images/`
- Dark mode transitions are hardware-accelerated

### Browser Support
Tested and optimized for:
- Chrome/Edge 90+
- Firefox 90+
- Safari 14+
- Mobile Safari iOS 14+

---
## Troubleshooting

### Font Installation Issues
- Ensure all font files are placed in the correct `/static/type/` directory
- Verify file permissions are set correctly
- Check browser console for 404 errors on font files

### Build Statistics Not Appearing
- Confirm GitHub Actions workflow is properly configured
- Verify Git integration is enabled in hugo.toml
- Check data/stats directory exists and has write permissions

### Drop Cap Rendering
- Chrome: May require additional CSS adjustments (included in theme)
- Firefox: Verify proper font loading
- Safari: Check for proper font-feature-settings

---
## Credits & License

### Fonts
* [Doves Type](https://typespec.co.uk/doves-type/) - Commercial font, purchase from Typespec
* [Fira Code](https://github.com/tonsky/FiraCode) - By [Nikita Prokopov](https://github.com/tonsky) - [GitHub](https://github.com/tonsky/FiraCode)

### Colors
* [Flexoki](https://github.com/kepano/flexoki) color scheme by [Steph Ango](https://github.com/kepano) - [GitHub](https://github.com/kepano/flexoki)

### License
MIT License - See LICENSE file for details

### Author
Diego Iaconelli
Original design and implementation for [Deadline](https://iaconelli.org)
