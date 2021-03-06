---
layout: post
title:  "Square to Hex"
date:   2017-03-11 00:00:00
categories: graphics
---

Imagine you had a square grid of points and you'd like to transform that grid into a hexagonal grid of points such that the local relationships between points are preserved. For irregular or arbitrary start or target grids tools like Mario Klingemann's [RasterFairy](https://github.com/Quasimondo/RasterFairy) do an excellent job. However for a regular to regular transform I was wondering if there were optimal, regular, tilable solutions.

The naive way to transform from square to hex is to simply take every second row and shift it by half the gridspacing. That gives you triangles and you're done. However, unfortunately, the triangles are now not equilateral, being stretched in y by sqrt(3)/2. Ok, you say, why not just just rescale in y by 2/sqrt(3), and now you're done. Ok, but now you've changed the *aspect ratio* of the points. What if you wanted to preserve the aspect ratio *and* get an equilateral, hexagonal arrangement *and* minimize the grid distortion.

Obviosuly there can't be perfect solutions to this problem since sqrt(3) is irrational.
However we can find grid arrangements whose aspect ratio change is very close to 1.0

| Square | Hexagonal | Aspect Devation |  Points  |
|--------|-----------|-----------------|----------|
|    4x4 |    4x4    |  0.866          |    20    |
|    4x6 |    4x6    |  0.866          |    24    |
|  16x14 |  14x16    |  1.131          |   224    |
|  18x16 |  16x18    |  1.096          |   288    |
|  20x18 |  18x20    |  1.069          |   360    |
|  22x20 |  20x22    |  1.048          |   440    |
|  24x22 |  22x24    |  1.031          |   528    |
|  26x24 |  24x26    |  1.016          |   624    |
|  28x26 |  26x28    |  1.004          |   728    |



The 28x26 solution appears to be so optimal that there appears to be no better solution up to 10000 points (i didn't search past that point. Eventually, of course, there should be an even better approximation though it becomes intractable for the assignment algorithm (see below) since the Kuhn-Munkres algorithm is O(N^3).

Ok, now once we have the grids we still have to assign which point in the square grid becomes which point in the hex grid.
We can calculate the ideal minimal distortion by solving [the assignment problem](https://en.wikipedia.org/wiki/Hungarian_algorithm) over a cost matrix of distances. For the patches to be tileable we need to calculate the distance matrix under periodic boundary conditions, i.e if you walk out of the unit cell to the left you reappear on the right.

## 16x14 = 224 points

<a type="text/html" href="/assets/squaretohex/solution.224.txt"><img src="/assets/squaretohex/solution.224.gif"></a>

### 18x16 = 288 points

<a type="text/html" href="/assets/squaretohex/solution.288.txt"><img src="/assets/squaretohex/solution.288.gif"></a>

## 20x18 = 360 points

<a type="text/html" href="/assets/squaretohex/solution.360.txt"><img src="/assets/squaretohex/solution.360.gif"></a>

## 22x20 = 440 points

<a type="text/html" href="/assets/squaretohex/solution.440.txt"><img src="/assets/squaretohex/solution.440.gif"></a>

## 24x22 = 528 points

<a type="text/html" href="/assets/squaretohex/solution.528.txt"><img src="/assets/squaretohex/solution.528.gif"></a>

## 26x24 = 624 points

<a type="text/html" href="/assets/squaretohex/solution.624.txt"><img src="/assets/squaretohex/solution.624.gif"></a>

## 28x26 = 728 points

<a type="text/html" href="/assets/squaretohex/solution.728.txt"><img src="/assets/squaretohex/solution.728.gif"></a>


## Tiling

All these can be tiled so arbitrarily large planes of points can be converted this way.:

### 224 2x2

<img src="/assets/squaretohex/solution.224.tile2x.gif">

### 728 2x2

<img src="/assets/squaretohex/solution.728.tile2x.gif">





