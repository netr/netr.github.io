+++
date = '2024-11-04T20:51:45-08:00'
draft = false
title = 'Creating a blog using Hugo and Github Pages'
+++

## Why I chose Hugo and Github Pages for this blog

I've been wanting to start a blog for a very long time. I've setup WordPress blogs, a Ghost blog, and --because i'm a developer-- i've definitely tried building my own blog. 

Since I never got in the habit of writing blogs enough, the hosting was never worth it. I'd get no traffic. 

With Github Pages, the hosting is taken care of for you. Hugo is completely open source, so it's free as well. This gave me the fastest option to go from idea to blog to hosting. Once it's setup, outside of the domain, it's always going to be live. I can build my habits without the cost.

It's really easy to setup. Took me about an hour. I easily added a custom theme I wanted (https://themes.gohugo.io/themes/hugo-theme-m10c/) and added all the customizations that I could into my `hugo.toml`. I was trying to learn how the configuration file worked, and this gave me the first chance to start turning knobs. 

With a tiny bit of color choosing, I had my blog ready to go.

## Setting up Hugo on Ubuntu 24.04

Installed hugo from the `go install` and installed `extended`. My theme needed to have the `scss` capabilities, so I needed extended to run it. 

Then i ran 
```bash
hugo new blog
cd /blog
hugo server
```

Just like that. I have something working. 

## Configuring the theme

Inside the folder you'll have `/themes/[theme_name]` -- inside of `/assets/css/components` i found all the .scss files that I needed. Your theme will obviously vary, but it's nice to see that I can customize every part of this very easily. That's another huge plug to hugo. There's not a huge amount of clutter in your way from customizing your site. It's made for developers and it works well for us.

## Fix for: WARN found no layout file for "html" for kind "home"

At one point during my testing I was having an issues seeing my first post. My theme wasn't working and showing the post. I was trying all kinds of things and after talking to claude. I realized i forgot to pass the theme in. Even though i was getting some visual feedback that the theme was being used, it wasn't working. If you see these warnings:

```text
WARN  found no layout file for "html" for kind "home": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.
WARN  found no layout file for "html" for kind "section": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.
WARN  found no layout file for "html" for kind "taxonomy": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.
WARN  found no layout file for "html" for kind "page": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.
```

Be sure to pass your theme in like `hugo server --theme=hugo-theme-m10c`

## Setting up a date separated url format for permalinks

To get a wordpress style permalink for your blog, you can take advantange of the `Front Matter` date and the `permalink` config:

```toml
[permalinks]
    [permalinks.page]
        blog = '/blog/:year/:month/:day/:slug/'
```

Turns your `/content/blog/2024/post.md` into `https://domani.com/blog/2024/11/04/post`

## Customizing the code block's theme and font

Here is a list of all the chroma styles: [Chroma Style Gallery](https://xyproto.github.io/splash/docs/)  and find all the stylings / options: [hugo transform highlight options](https://gohugo.io/functions/transform/highlight/#options)

You can easily customize it in the `hugo.toml` using:

```toml
[markup]
    [markup.highlight]
        codeFences = true
        lineNoStart = 1
        lineNos = true
        noClasses = true
        style = "catppuccin-mocha"
```

If you want to have more control over the css you can run `hugo gen chromastyles --style=monokai > syntax.css` and change the config to:

```toml
[markup]
    [markup.highlight]
        lineNoStart = 1
        lineNos = true
        noClasses = false
        codeFences = true
```

Move the `syntax.css` to something like `/static/css/syntax.css` and import it into your layout using `<link rel="stylesheet" href="/css/syntax.css">`.

![Overflow mess](/images/fix-to-overflow-mess.webp)

This is the route I chose because I was having overflow problems on the `Fix for:` code block above. It was running all of the text off the page. To fix it inside of my `static/css/syntax.css` I changed:

```css
/* PreWrapper */ .chroma { color:#cdd6f4;background-color:#1e1e2e;overflow:auto; }
```

## Writing, this, my first blog post

`hugo create content/blog/2024/blog-title.md`

It uses [front matter](https://gohugo.io/content-management/front-matter/) which allows you to create metadata for each blog. You can create default templates inside of [archetypes](https://gohugo.io/content-management/archetypes/) but I didn't get into that much. They're very robust and good to know that they're there. 

## Pushing to github

Since i'm using my username, i created the repository `netr.github.io` and uploaded my files to it. I've seen online that some people use submodules for their public folder. I might do that later, but for now i'm going to keep it simple. 

## Set up continuous deployment onto github pages using github actions

Using the hugo workflow, I ran into some problems initially. Github actions was failing while trying to build the theme with jekyll. I had to pull the them into the project and remove any .git bindings. 

If github pages is only returning an XML. Check the github action logs for `WARN found no layout file`. If you're getting them, like I was, add the theme to your github actions command:
```yaml
    steps:
      - name: Install Hugo CLI
    ...
    ...
    run: |
        hugo \
        --gc \
        --minify \
        --theme=hugo-theme-m10c \ <-- add this line here
        --baseURL "${{ steps.pages.outputs.base_url }}/"
```

## Setting custom domain

## Speed tests

Out of the box while viewing this blog post, it got:

![Baseline performance](/images/github-pages-performance.webp)

## Problems in Accessibility
 
``` 
Serve images in next-gen formats Potential savings of 51 KiB
```

![Webp vs Png](/images/webp-vs-png-filesize.webp)

I converted all the images to webp, pushed to github and tried again:

Accessibility fix for: Background and foreground colors do not have a sufficient contrast ratio.

I fixed it by changing the `sd` and `comment` of my code block class to a brighter color. The contrast of the line numbers, comments, and command line text was not high enough.

To fix the links, I just added `text-decoration: underline`. This brough the accessbility back up to 100%.

and 

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&family=JetBrains+Mono:ital,wght@0,100..800;1,100..800&display=swap" rel="stylesheet">
```

```
Eliminate render-blocking resources Potential savings of 280 ms
```

There's still some issues with using the google fonts, but I like how they look. It's worth a bit of a delay for them. 

## It's easy enough, if you want to start a blog, this is the most frictionless