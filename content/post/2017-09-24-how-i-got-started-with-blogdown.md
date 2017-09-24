---
title: How I got started with `blogdown`
author: Gabi Huiber
date: '2017-09-24'
slug: how-i-got-started-with-blogdown
categories: []
tags: []
description: ''
---

The first thing to notice after you follow [Yihui's instructions](https://bookdown.org/yihui/blogdown/a-quick-example.html) from the beginning to section 1.2 is that the generic `config.toml` is the same as the one in `exampleSite`:

```
diff config.toml themes/hugo-lithium-theme/exampleSite/config.toml
```

So if you're going to experiment with different themes, it's probably best to do it now before you go down too deep into modifying `config.toml` to suit your needs. The best way to adopt new themes, it seems to me, is to follow the recipe from [Minimal](https://github.com/calintat/minimal) everywhere: add each new theme as a Git submodule. This works as shown if your themes come from GitHub.