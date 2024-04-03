---
title: "Render Math With Mathjax"
date: 2024-04-03
draft: false
author: "Michal Šrámek"
devto: "https://dev.to/michalsramek/render-math-with-mathjax-4j0m"
tags:
  - Markdown syntax
  - Mathjax
  - example
image: /images/blogs/mathjax.png
description: "How to beat MATH"
toc: true
mathjax: true
---

## Mathjax

Math equations can be rendered using [Mathjax](https://www.mathjax.org) syntax with AMS symbol support.

Optionally enable this on a per-page basis by adding `mathjax: true` to your frontmatter.

Then, use `$$ ... $$` on a line by itself to render a block equation:

$$ | Pr_{x \leftarrow P_{1}} [A(x) = 1] - Pr_{x \leftarrow P_{2}} [A(x) = 1] | < \text{negligible} $$

The raw version is:

```
$$ | Pr_{x \leftarrow P_{1}} [A(x) = 1] - Pr_{x \leftarrow P_{2}} [A(x) = 1] | < \text{negligible} $$
```


Write in-line equations with `\\( ... \\)` , like \\( x^n / y \\) . It's easy!

```
Write in-line equations with `\\( ... \\)` , like \\( x^n / y \\) . It's easy!
```

