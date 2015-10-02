---
layout: post
title:  "Experiments with style transfer"
date:   2015-10-02 00:03:44
categories: code
---

Since the original [Artistic style transfer](http://arxiv.org/pdf/1508.06576v1.pdf) and the subsequent [Torch implementation of the algorithm](https://github.com/jcjohnson/neural-style) by Justin Johnson were released I've been playing with various ways to use the algorithm in other ways. Here's a quick dump of the results of my experiments. None of these are particularily rigorous and I think there's plenty of room for improvement.

## Super resolution

I was wondering if you could use style transfer to create super resolution images. I.e. use a low resolution image of something as the content image and a high-resolution image of something similar as "style". The result looks as follows:

<center><img src="/assets/obama_nicholson_comparison_125.png"></center>

Left: High-res image of Obama
Right: Low-res image of Nicholson
Middle: After style transfer

Conclusion: Yeah, sort of. You get some interesting artefacts.

## Style drift

One of the things that struck me about the results of style transfer (see for example this excellent, [comprehensive experiment by Kyle McDonald](http://kylemcdonald.net/stylestudies/) ) is that a single iteration of style transfer mostly transfers texture, i.e. high frequency correlation. The result still maintains most of the exact geometries of the photo, but painted with similar visual components from the style image. 

In this style drift experiment i basically apply different styles iteratively to let the image drift from it's original. I.e. say you have 4 styles, you apply 1 then 2 then 3 then 4 then 1 again and so forth. Each style pushes in a different direction - like a game of telephone - by the time you get back to the same style the image is significantly (but not randomly) distorted. It's an interesting way to remix different aspects of different styles. 

<center><img src="/assets/style_drift_montage.jpg"></center>

The drift is more striking when viewed as a video:

<iframe width="420" height="315" src="https://www.youtube.com/embed/-BpGMHTKTKA" frameborder="0" allowfullscreen></iframe>
<br>
<iframe width="420" height="315" src="https://www.youtube.com/embed/dy6JeQjwLNk" frameborder="0" allowfullscreen></iframe>



