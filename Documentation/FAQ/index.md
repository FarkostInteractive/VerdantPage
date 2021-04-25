---
layout: default
title: "FAQ"
nav_order: "4"
---

# FAQ
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Terrain

### Q. I just changed the graphics API in Project Settings and all the vegetation on my terrain disappeared. How can I get it back?
A. Do a small edit to the terrain (and undo it if you need to). Then save, reload Verdant and the vegetation should come back. This is a bug that exists in some versions of Unity where scripts reading the terrain heightmap will receive an all-black texture after the API has changed. It only happens right after the change and fixing it once should prevent it from happening again. 

## Masks

### Q. I swapped from gamma to linear rendering in Project Settings and now all my masks look different
A. You probably haven't unchecked the sRGB (Color Texture) parameter on your mask textures. It is set by default, so you need to do this manually. When the color space is set to linear Unity will apply gamma correction to any textures that have it set, and that will change the value of the texture when Verdant reads it.