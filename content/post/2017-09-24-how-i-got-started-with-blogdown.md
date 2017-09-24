---
title: How I got started with blogdown
author: Gabi Huiber
date: '2017-09-24'
slug: how-i-got-started-with-blogdown
categories: []
tags: []
description: ''
---

The first thing to notice after you follow [Yihui's instructions](https://bookdown.org/yihui/blogdown/a-quick-example.html) from the beginning to section 1.2 is that the generic `config.toml` is the same as the one in `exampleSite`. You can do that in the Bash shell with `diff`, which returns nothing if the two files are identical:

```
diff config.toml themes/hugo-lithium-theme/exampleSite/config.toml
```

So if you're going to experiment with different themes, it's probably best to do it now before you go down too deep into modifying `config.toml` to suit your needs. The cleanest way to adopt new themes, it seems to me, is to follow the recipe from [Minimal](https://github.com/calintat/minimal) everywhere: add each new theme as a Git submodule. This way you can update a theme easily as soon as the author tinkers with it. This works as shown for any theme that comes from GitHub, as far as I can tell. I used this recipe for [Ghostwriter](https://github.com/jbub/ghostwriter) and [Pickles](https://github.com/mismith0227/hugo_theme_pickles).

That this Git module business works doesn't mean that the theme suits blogdown: Ghostwriter does, but Pickles does not. So, nevermind. You can remove a Git submodule using instructions [here](https://stackoverflow.com/questions/1260748/how-do-i-remove-a-submodule).