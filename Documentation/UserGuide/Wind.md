---
layout: default
title: "Wind"
parent: "User Guide"
grand_parent: "Documentation"
nav_order: "1"
---

# Wind

## Wind Volumes

When you add Verdant to a scene the wind will be completely still. To change it you need to create a GameObject with a Wind Volume component.

As the name implies, a Wind Volume is a volume within which the wind behaves a certain way. “Within” here does not mean the vegetation instances inside the volume. Rather, wind is applied globally when the CAMERA enters a volume. This distinguishes the wind system from the deflection field. Deflection allows for fine local detail whereas wind is an ambient effect applied to all vegetation.

Volumes can be nested and will transition smoothly between one another depending on the transition time parameter. This allows you to have one large global wind volume and smaller local zones within it. 

You can also control wind parameters from a script. See the scripting reference for details.

## Wind parameters
### Wind Direction:
Controls the direction of the wind. You can snap to 15 degree intervals by holding down ——— as you change it.

### Wind Strength:
Wind strength follows the ____ Scale, which classifies wind based on observable effects rather than an exact measurement like meters per second. Use the big slider to choose the Buefort stage, which is displayed on the labels around the smaller slider. These have names like Gale, Hurricane, Fresh Breeze, and so on. On the left is the one chosen, on the right the next step on the scale. The smaller slider can be used to interpolate between the two. 

### Gust Texture
This texture is used to add gusts traveling along the field. Most of the time there’s no need to change it from the default. If you do change it, it’s worth having a look at the default to give you a sense of how to make your own. You want to use a repeating noise texture, and the sharper your noise peaks are the more distinct your gusts will be. 

### Gustiness
This is a simple multiplier for the gust texture. It can be used to completely disable gusts or to make them stronger, which can make for a more dramatic scene. 

## Stiffness 
Verdant Types have a parameter called stiffness that determines how they interact with wind. The stiffer the type the stronger the wind needed to bend it. 

## Accessing wind from other shaders
If you have other vegetation in your scene you can access the Verdant wind system by making it a global shader parameter. The page Accessing Verdant Data details how to do this and the shader functions you need to use. 