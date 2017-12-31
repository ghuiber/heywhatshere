---
title: Hugo themes and Netlify
author: Gabi Huiber
date: '2017-12-31'
slug: hugo-themes-and-netlify
categories: []
tags: []
---

If you install a theme as a Git submodule as shown [here](https://github.com/calintat/minimal) and deploy your site on Netlify with the "New site from Git" button, Netlify may protest that it cannot find the remote repo for your theme. This is because it does not have access to it. 

Forking the original repo and pointing your remote to your fork as the origin, like I did, won't be enough of a fix, either. You will want to take a look at the `.gitmodules` file and update the url you see there. In my case, this means that when my first deploy failed I had to change this:

```
[submodule "themes/minimal"]
  path = themes/minimal
  url = https://github.com/calintat/minimal.git 
```

To this:

```
[submodule "themes/minimal"]
  path = themes/minimal
  url = https://github.com/ghuiber/minimal.git 
```
