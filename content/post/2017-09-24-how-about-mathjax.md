---
title: How about MathJax?
author: Gabi Huiber
date: '2017-09-24'
slug: how-about-mathjax
categories: []
tags: []
---

The default Lithium theme in blogdown comes with MathJax support. This big Sigma here -- `\(\Sigma\)` -- will show like the Greek upper case Sigma if you have it.

This theme is very nice but if you want to use another, two good candidates are [Minimal](https://github.com/calintat/minimal) and [Ghostwriter](https://github.com/jbub/ghostwriter). To make them useful for occasional mathematics typesetting, you will want to enable MathJax support there too.

The Lithium theme comes with a script called `math-code.js` here:


``` r
library('here')
```

```
## here() starts at /Users/ghuiber/heywhatshere
```

``` r
dir(file.path(here(), 'themes/hugo-lithium-theme/static/js'))
```

```
## [1] "math-code.js"
```

I copied this script to Ghostwriter's `static/js` folder, and I copied the whole `js` folder to Minimal's `static` folder at the command line:

```
cp themes/hugo-lithium-theme/static/js/math-code.js themes/ghostwriter/static/js/
cp -R themes/hugo-lithium-theme/static/js themes/minimal/static/
```

I had to edit the other themes' respective `config.toml` files with some extra parameters, so I  first made sure to preserve the originals:

```
cp themes/ghostwriter/exampleSite/config.toml themes/ghostwriter/exampleSite/config_example.toml
cp themes/minimal/exampleSite/config.toml themes/minimal/exampleSite/config_example.toml
```

Then I copied over this section of the `[params]` block from the Lithium `config.toml` file to the `[params]` block in each of the other themes' own `config.toml` file. Notice that this block handles both code highlighting and MathJax. I want to take care of both at once. If this screws anything up, I'll just revert to the `config_example.toml` that I saved:

```
    # options for highlight.js (version, additional languages, and theme)
    highlightjsVersion = "9.11.0"
    highlightjsCDN = "//cdn.bootcss.com"
    highlightjsLang = ["r", "yaml"]
    highlightjsTheme = "github"

    MathJaxCDN = "//cdn.bootcss.com"
    MathJaxVersion = "2.7.1"
```

In addition to the `math-code.js` script in the `static/js` folder and the parameters in `config.toml`, Hugo needs to see these pieces called from inside one of the html files in `layouts/partials`. 

## How does Lithium do it?

The Lithium theme has `footer_mathjax.html`, `footer_highlightjs.html` and `header_highlightjs.html` in the `partials` folder.

Here's the bottom of `themes/hugo-lithium-theme/layouts/partials/footer.html`:

```
    {{ partial "footer_highlightjs" . }}
    {{ partial "footer_mathjax" . }}
    {{ template "_internal/google_analytics.html" . }}
  </body>
</html>
```

And here's the bottom of `themes/hugo-lithium-theme/layouts/partials/head.html`:

```
{{ partial "head_highlightjs" . }}
<link rel="stylesheet" href="{{ "css/fonts.css" | relURL }}" media="all">
<link rel="stylesheet" href="{{ "css/main.css" | relURL }}" media="all">
{{ range .Site.Params.customCSS }}
<link rel="stylesheet" href="{{ . | relURL }}">
{{ end }}
{{ partial "head_custom" . }}
```

This `head.html` file is called inside the `<head></head>` tags of `header.html` with a partial call. Here's that file: 

```
<!DOCTYPE html>
<html lang="{{ .Site.LanguageCode }}">
  <head>
    {{ partial "head.html" . }}
  </head>
  <body>
    <div class="wrapper">
      <header class="header">
        {{ partial "nav.html" . }}
      </header>
```

So: the code in `head_highlightsjs.html` needs to make its way into the head section of `header.html` and the Lithium theme is doing this through an indirect call: `header.html` calls `head.html` which then calls `head_highlightjs.html`. OK.

## How about the other two templates?

Neither Minimal nor Ghostwriter have `footer_mathjax.html`, `footer_highlightjs.html` and `header_highlightjs.html` in the `partials` folder so I'll start by copying them over in both places. 

## How might Ghostwriter use these three files?

The Ghostwriter theme handles calls to its own .js files from inside `footer.html`. Here's the bottom of `themes/ghostwriter/layouts/partials/footer.html`:

```
        <script src="{{ .Site.BaseURL }}js/scripts.js"></script>
    </body>
</html>
```

So I could replace the above with

```
        <script src="{{ .Site.BaseURL }}js/scripts.js"></script>
        {{ partial "footer_highlightjs" . }}
        {{ partial "footer_mathjax" . }}
    </body>
</html>
```

I also need to make a call to `header_highlightjs.html` from Ghostwriter's own `header.html` partial and make it inside the `<head></head>` tags. So here's the relevant piece of `themes/ghostwriter/layouts/partials/header.html`:

```
        {{ if .RSSLink }}
            <link href="{{ .RSSLink }}" rel="alternate" type="application/rss+xml">
        {{ end }}
    </head>
```

And I'm going to change it to

```
        {{ if .RSSLink }}
            <link href="{{ .RSSLink }}" rel="alternate" type="application/rss+xml">
        {{ end }}
        {{ partial "head_highlightjs" . }}
    </head>
```

## How about Minimal?

The Minimal theme -- which didn't come with a `static/js` folder of its own -- has a `js.html` file that calls some .js files from various locations on the internets. So, I could use the same procedure: copy over three partials, call two of them from the footer, one from the header. 

## Conclusion

This worked: with these modifications both Minimal and Ghostwriter have MathJax enabled. This will do for an exploration of other themes. I decided to keep Lithium.
