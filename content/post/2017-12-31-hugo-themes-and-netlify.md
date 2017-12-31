---
title: Hugo themes and Netlify
author: Gabi Huiber
date: '2017-12-31'
slug: hugo-themes-and-netlify
categories: []
tags: []
---

If you install a theme as a Git submodule as shown here, your Netlify deploy with the “New site from Git” button may fail. This happens when Netlify does not have access to the original repo for your theme. 

My solution was to update the url in the `.gitmodules` file. After my first deploy failed I changed this:

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
