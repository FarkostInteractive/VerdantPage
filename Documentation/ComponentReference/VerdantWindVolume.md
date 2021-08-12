---
layout: default
title: "VerdantWindVolume"
parent: "Component Reference"
nav_order: "6"
---

# VerdantWindVolume

A component used to create local wind configurations. When the [VerdantCamera](VerdantCamera) enters the volume its settings will be applied to all the vegetation in the scene.

Wind strength is set using the [Beaufort scale](https://en.wikipedia.org/wiki/Beaufort_scale), which has a long history of being used at sea and is based on observation more than exact measurement. It comes with convenient labels in plain english for each of its twelve steps, which are shown on both sides of the big slider. The small slider below it lets you select an intermediary value between two steps. 

Wind volumes can be nested. Verdant keeps track of all the volumes the camera enters and exits, making it so the last entered volume always takes precedence over volumes entered previously. When exiting a nested volume Verdant automatically returns the wind configuration to the larger volume (given that the camera is still inside it). It's also possible to overlap volumes such that a player enters through volume A and then exits it while inside volume B. When the camera exits volume B in this case it won't return to configuration A.

When the camera starts inside a nested volume Verdant will ensure that the smallest volume is applied last. For this to work the smaller volume must be completely contained by the larger volume. If the volumes only overlap partially Verdant has no way to know which of them the camera theoretically entered first, so it will pick one arbitrarily. Try to avoid starting in intersections between volumes.

It's possible to change all wind parameters from script. As with many other Verdant components you need to apply them by calling SetDirty(). If the camera is inside the volume when it is changed the user will see them applied to the scene vegetation instantly. As changing wind parameters is cheap you can use this functunality to animate the wind at runtime however you like. 

Wind volumes are used to change global wind as the camera moves through the scene. For local wind phenomena like gusts or whirlwinds you can use a [deflection affector](Affectors/VerdantDeflectionAffector.html).

## Parameters

|:---------------|:--------------------------|
| `Extents` | The extents of this volume, which is half of its total size along any given axis. Will be multiplied by the transform scale. |
| `Transition Time` | The amount of time it takes to fade into this volume from the prior wind state. |
| `Beaufort Stage` | The discrete step along the Beaufort scale. In the inspector this is represented by the big slider with numbers 0-12 along it. |
| `Beaufort Interpolation` | A value between 0 and 1 that is added to the Beaufort stage to get an intermediary value. |
| `Turbulence` | Controls how much the wind strength varies over time. Higher turbulence causes vegetation to flutter more. |
| `Gustiness` | Controls how strong the gusts created using the Wind Noise Texture are. It scales with the general wind strength. |
| `Wind Direction` | A 2D vector that controls the direction of the wind in the XZ plane.  |
| `Wind Noise Texture` | A greyscale texture used to create gust patterns traveling along the ground.  |

## Public Methods

|:---------------|:--------------------------|
| `void SetDirty()` | Notifies the VerdantCamera that this volume has changed. If the camera is inside it the wind in the scene will update. Must be called for parameter changes to have effect. |