---
layout: default
title: "VerdantWindVolume"
parent: "Component Reference"
nav_order: "6"
---

# VerdantWindVolume



Wind strength is set using the [Beaufort scale](https://en.wikipedia.org/wiki/Beaufort_scale), a scale for wind that is based on observation more than exact measurement. It comes with convenient recognizable labels in english for each of its twelve steps, which are shown on both sides of the big slider. The small slider below it lets you select an intermediary value between the two steps. 

## Parameters

|:---------------|:--------------------------|
| `Extents` | The extents of this volume, which is half of its total size along any given axis. Will be multiplied by the transform scale. |
| `Transition Time` | The amount of time it takes to fade into this volume from the prior wind state. |
| `Beaufort Stage` | The discrete step along the Beaufort scale. In the inspector this is represented by the big slider with numbers 0-12 along it. |
| `Beaufort Interpolation` | A value between 0 and 1 that is added to the Beaufort stage to get an intermediary value. |
| `Gustiness` | Controls how strong the gusts created using the Wind Noise Texture are. It scales with the general wind strength. |
| `Wind Direction` | A 2D vector that controls the direction of the wind in the XZ plane.  |
| `Wind Noise Texture` | A greyscale texture used to create gust patterns traveling along the ground.  |

## Public Methods

|:---------------|:--------------------------|
| `void SetDirty()` | Notifies the VerdantCamera that this volume has changed. If the camera is inside it the wind in the scene will update. Must be called for parameter changes to have effect. |