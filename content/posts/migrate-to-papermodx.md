---
title: "Migrate Hugo From PaperMod Theme to PaperModX"
date: 2025-08-13
ShowToc: false
---

## Introduction

I've just switched my Hugo theme from [PaperMod](https://github.com/adityatelange/hugo-PaperMod) to [PaperModX](https://github.com/reorx/hugo-PaperModX) for the features listed on their repository.
The steps to do so are pretty straightforward and listed below.

## Migration Steps

First, add the theme as a sub-module.

```sh
git submodule add https://github.com/reorx/hugo-PaperModX.git themes/PaperModX
```

Then, change the Hugo theme to `PaperModX`.

```sh
sed -i '' 's/^theme: PaperMod$/theme: PaperModX/' hugo.yaml
```

Finally, delete the now unused `PaperMod` sub-module.

```sh
git rm themes/PaperMod
```
