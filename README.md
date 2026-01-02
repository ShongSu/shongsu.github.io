# A Piece of Sunshine

Personal blog powered by Jekyll and hosted on GitHub Pages.

## License

All the original content (posts and pages) is Copyright Pengyu Chen.

All other directories and files are MIT Licensed. Feel free to use the HTML and CSS as you please but I would greatly appreciate if you put a little bit of effort into changing the colors and look of your site.

## Setup

### Prerequisites

- Ruby (>= 2.7.0)
- Bundler gem

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/Shongsu/shongsu.github.io.git
   cd shongsu.github.io
   ```

2. Install dependencies:
   ```bash
   bundle install
   ```

3. Build and serve locally:
   ```bash
   bundle exec jekyll serve
   ```

4. Browse to:
   ```
   http://localhost:4000
   ```

## Creating a New Post

Create a new file in the `_posts` directory with the following naming convention:
```
YYYY-MM-DD-post-title.markdown
```

Add front matter at the top:
```yaml
---
layout: post
title: "Your Post Title"
date: YYYY-MM-DD HH:MM
tags: [tag1, tag2]
---
```

## Deployment

This site is automatically deployed to GitHub Pages when you push to the `main` or `master` branch.

## Development

- Jekyll version: 4.3+
- Theme: Clean Blog (customized)
- Syntax highlighting: Rouge
