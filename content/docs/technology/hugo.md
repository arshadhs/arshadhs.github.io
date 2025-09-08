---
title: 'Hugo Notes'
date: 2025-07-22T19:29:34+01:00
draft: false
tags: ["hugo"]
categories: ["hugo"]
---

# Hugo Notes

## Git Ignore

```
.hugo_build.lock
node_modules/
public/
resources/Add comment
```

## Theme Switch
To reuse a theme as a sub-module, when seeing following error:
fatal: A git directory for 'themes/hugo-book' is found locally with remote(s):
  origin        https://github.com/alex-shpak/hugo-book
If you want to reuse this local git directory instead of cloning again from
  https://github.com/alex-shpak/hugo-book
use the '--force' option. If the local git directory is not the correct repo
or you are unsure what this means choose another name with the '--name' option.

> rm -rf .git/modules/blah
> git submodule add git://path.to.new

## Line Ending Warnings
To fix git Warning:
warning: in the working copy of '.gitmodules', LF will be replaced by CRLF the next time Git touches it

> git config --global core.autocrlf false

## Raw HTML

To fix: WARN  Raw HTML omitted while rendering; see rendererunsafe 

https://gohugo.io/getting-started/configuration-markup/#rendererunsafe

You can suppress this warning by adding the following to your site configuration:
ignoreLogs = ['warning-goldmark-raw-html']

## Git Submodule update

git submodule foreach git pull origin master

## Reclone and set-up Hugo

Delete and recreate the Hugo site after cloning:

```git clone https://github.com/xxx/abc.github.io
cd abc.github.io
hugo new site . --force
```

and set-up the workflow again

---
{{< home-link "Home" >}} | {{< section-index >}}