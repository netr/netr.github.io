+++
date = '2024-11-04T20:51:45-08:00'
draft = false
title = 'Creating a blog using Hugo and Github Pages'
description = "Why and how to create a blog using Hugo and hosting it for free on Github Pages."
+++

## Why I chose Hugo and Github Pages for this blog

I've been wanting to start a blog for a very long time. I've setup WordPress blogs, a Ghost blog, and --because i'm a developer-- i've definitely tried building my own blog. 

Since I never got in the habit of writing blogs enough, the hosting was never worth it. I'd get no traffic. 

With Github Pages, the hosting is taken care of for you. Hugo is completely open source, so it's free as well. This gave me the fastest option to go from idea to blog to hosting. Once it's setup, outside of the domain, it's always going to be live. I can build my habits without the cost.

## Getting Started with Hugo

### Setting up Hugo on Ubuntu 24.04

1. Install Hugo extended version using `go install` (the extended version is needed for SCSS capabilities)
2. Create a new blog:
```bash
hugo new blog
cd /blog
hugo server
```

### Choosing and Configuring a Theme

1. Browse themes at https://themes.gohugo.io/
2. I chose the m10c theme (https://themes.gohugo.io/themes/hugo-theme-m10c/)
3. Configure the theme in `hugo.toml`
4. Customize theme colors and styles in `/themes/[theme_name]/assets/css/components/*.scss`

### Common Theme Setup Issues

If you see these warnings:
```text
WARN  found no layout file for "html" for kind "home"
WARN  found no layout file for "html" for kind "section"
WARN  found no layout file for "html" for kind "taxonomy"
WARN  found no layout file for "html" for kind "page"
```
Solution: Pass your theme explicitly when running the server:
```bash
hugo server --theme=hugo-theme-m10c
```

## Configuring Your Blog

### Setting up URL Structure

To get WordPress-style permalinks, configure your `hugo.toml`:
```toml
[permalinks]
    [permalinks.page]
        blog = '/blog/:year/:month/:day/:slug/'
```
This transforms `/content/blog/2024/post.md` into `https://domani.com/blog/2024/11/04/post`

### Customizing Code Blocks

1. View available styles at [Chroma Style Gallery](https://xyproto.github.io/splash/docs/)
2. Configure in `hugo.toml`:
```toml
[markup]
    [markup.highlight]
        lineNoStart = 1
        lineNos = true
        noClasses = false
        codeFences = true
```

For more control:
1. Generate custom CSS: `hugo gen chromastyles --style=monokai > syntax.css`
2. Move to `/static/css/syntax.css`
3. Import in layout: `<link rel="stylesheet" href="/css/syntax.css">`
4. Fix overflow issues by adding:
```css
/* PreWrapper */ .chroma { color:#cdd6f4;background-color:#1e1e2e;overflow:auto; }
```

## Creating Content

Create new posts using:
```bash
hugo create content/blog/2024/blog-title.md
```
Posts use [front matter](https://gohugo.io/content-management/front-matter/) for metadata. You can create default templates in [archetypes](https://gohugo.io/content-management/archetypes/).

## Deployment

### Setting up Github Pages

1. Create a repository named `username.github.io`
2. Upload your Hugo files
3. Set up Github Actions for continuous deployment

### Github Actions Configuration

If Github Pages only returns XML, check action logs for `WARN found no layout file`. Fix by adding the theme to your workflow:
```yaml
steps:
  - name: Install Hugo CLI
    ...
    run: |
        hugo \
        --gc \
        --minify \
        --theme=hugo-theme-m10c \
        --baseURL "${{ steps.pages.outputs.base_url }}/"
```

## Lighthouse Tests and Optimizations

Initial lighthouse test results showed room for improvement. This is still very good out of the box, especially for how easy it was to setup. All the required fixes were my fault rather than anything hugo was doing. 

![Baseline performance](/images/github-pages-performance.webp)

### Optimizing Images 
- Converted images to WebP format
- Reduced file sizes significantly

![Webp vs Png](/images/webp-vs-png-filesize.webp)

### Optimizing Fonts
Combined fonts into one url and used preconnects to shave off some time by establishing a connection early.
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&family=JetBrains+Mono:ital,wght@0,100..800;1,100..800&display=swap" rel="stylesheet">
```

### Added meta description to blog post
The SEO was taking a 9 point hit because I forgot to add a meta description. Since hugo uses front matter, all I had to do was add a description tag to the top of the page.
```toml
description = "Why and how to create a blog using Hugo and hosting it for free on Github Pages."
```

### Improving Accessibility

![Starting accessibility](/images/contract-accessibility.webp)

1. Fixed contrast ratios for code blocks. Google devtools has a great feature that highlights the exact html blocks that are giving you issues. As seen here: [comment](/images/comment.webp), [page number](/images/page-number.webp), [cli-text](/images/cli-text.webp)

2. Added an underline to all links so I didn't have to change the color and deal with contrast.

### Added a favicon

The `Best Practices` was complaining about errors in the console. It turned out that I was missing a favicon.ico. I converted my avatar into [an icon file](https://cloudconvert.com/) and that put me back at 100%. 

### Final Results
After optimizations, achieved near-perfect Lighthouse scores:
![100% on almost all lighthouse](/images/100-pct-lighthouse.webp)

Note: For best benchmark results, test in Chrome incognito mode to avoid extension interference.