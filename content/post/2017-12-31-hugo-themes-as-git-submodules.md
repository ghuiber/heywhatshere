---
title: Hugo themes as Git submodules
author: Gabi Huiber
date: '2017-12-31'
slug: hugo-themes-as-git-submodules
categories: []
tags: []
---

## Submodules

If you install extra themes -- such as Minimal -- as Git submodules following instructions [here](https://github.com/calintat/minimal) their remotes will point to the originals. You will want instead to fork the originals, then install _your forks_ as submodules. In other words, replace the first line of the original instructions from this:

```
$ git submodule add https://github.com/calintat/minimal.git themes/minimal
```

to this:

```
$ git submodule add https://github.com/<YOUR_GITHUB_ID>/minimal.git themes/minimal
```

This command, in the background, will write your `.gitmodules` entries accordingly. Instead of this (1):

```
[submodule "themes/minimal"]
  path = themes/minimal
  url = https://github.com/calintat/minimal.git 
```

you will see this (2):

```
[submodule "themes/minimal"]
  path = themes/minimal
  url = https://github.com/<YOUR_GITHUB_ID>/minimal.git 
```

This change will be useful to Netlify later.

Next, you will also want to set the original repos as `upstream` remotes. You can now make any changes you want, push them to your own fork, and maintain them that way. There's even an easy way to pull them all at once. It is described [here](https://stackoverflow.com/questions/1030169/easy-way-to-pull-latest-of-all-git-submodules). If you screw anything up, you can discard all changes and pull afresh from upstream as explained [here](https://stackoverflow.com/questions/13781388/git-discard-all-changes-and-pull-from-upstream).

My own remotes look like this:

```
Gabis-New-MacBook-Pro:heywhatshere ghuiber$ git remote -v
origin	git@github.com:ghuiber/heywhatshere.git (fetch)
origin	git@github.com:ghuiber/heywhatshere.git (push)
Gabis-New-MacBook-Pro:heywhatshere ghuiber$ cd themes/ghostwriter/
Gabis-New-MacBook-Pro:ghostwriter ghuiber$ git remote -v
origin	git@github.com:ghuiber/ghostwriter.git (fetch)
origin	git@github.com:ghuiber/ghostwriter.git (push)
upstream	https://github.com/jbub/ghostwriter.git (fetch)
upstream	https://github.com/jbub/ghostwriter.git (push)
Gabis-New-MacBook-Pro:ghostwriter ghuiber$ cd ../minimal/
Gabis-New-MacBook-Pro:minimal ghuiber$ git remote -v
origin	git@github.com:ghuiber/minimal.git (fetch)
origin	git@github.com:ghuiber/minimal.git (push)
upstream	https://github.com/calintat/minimal.git (fetch)
upstream	https://github.com/calintat/minimal.git (push)
```

## Netlify

If you log into Netlify with your GitHub credentials and deploy your Hugo blog with the “New site from Git” button, all should go without a hitch. What prompted this post is that my first deploy failed because I had followed to the letter the instructions for adding a submodule. Though I forked the original repos and I changed the remotes -- `origin` and `upstream` -- as shown above, the changes did not carry over to the `.gitmodules` entries, which pointed to the original authors' repos as shown in (1). This left Netlify with the impression that it did not have access to my `origin` remote repos. My immediate solution was to change the `.gitmodules` entries by hand. This did fix the deploy problem, but the easy solution is to fork first, then change what you put after `git submodule add` accordingly. That will write the corresponding `.gitmodules` entry correctly in the first place and Netlify will deploy without complaints.