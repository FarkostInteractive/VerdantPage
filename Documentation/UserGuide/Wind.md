---
layout: default
title: "Wind"
parent: "User Guide"
grand_parent: "Documentation"
nav_order: "3"
---

# Wind

## Table of contents
{: .no_toc .text-delta }

We already touched briefly on wind in getting started, but it's such an important part of designing a scene that it's well worth taking a deeper dive. We'll look closer at VerdantWindVolume, its parameters and how it can be used in your worlds.

## Wind Volumes

The first important thing to understand about wind is that it is global. All vegetation in the scene has the same amount of wind applied to it, but it's possible to change the wind configuration as the player enters different areas by using wind volumes. By default there is no wind in the scene, so you'll usually want to add at least one. Each volume contains a configuration that is applied as the camera enters it. You can use the Transition Time parameter to set how long it should take to fade into a configuration. The size of the volume is controlled both by the Bounds parameter and by the transform scale, which gets multiplied into the bounds. 

The main parameter of a wind volume is the strength of the wind, which is measured using the [Beaufort scale](https://en.wikipedia.org/wiki/Beaufort_scale). You'll notice that there is a big slider with 12 discrete steps and that beneath it is a smaller slider with labels on each end. The Beaufort scale was designed to be used at sea and measurable by eye, so each stage has a familiar name in plain english. The small slider is used to interpolate between two steps, so if you set the main step to 'gale' and slide it to 0.5 you'll get a force halfway between 'gale' and 'strong gale'. 

To give a bit more life to the wind we can use a wind noise texture. This is a repeating greyscale texture that will scroll through the world (in the XZ plane) and add some extra wind wherever the texture is bright. With the right texture the effect will be like little gusts passing through your vegetation. Verdant includes *****, which is ideal for choppy wind. It's a bit stringy and cellular with sharp transitions between black and white, which translates into clear and distinct gusts sweeping along the ground. The strength of the texture is controlled partially by the wind strength, but you can also increase Gustiness to give it a little boost. It can be necessary to do so when the base wind strength is low. 

Finally the wind direction compass controls both the direction of the wind and the gusts. You can set it directly by moving the needle (holding ctrl will make it snap to 30 degree intervals) or by entering a 2D vector in the fields below it.

A useful property of VerdantWindVolumes is that they can be nested. The current configuration will always be from the last volume that was entered, so it's possible to enter while inside another volume. Exiting will take you back to the first configuration again. It will even keep track of volumes that only partially intersect!

![Image of intersecting volumes]()

In this example, the player will enter volume A first and then volume B. While inside B they leave A before exiting both. The wind will configure to A, then to B, and as the player exits B Verdant will understand that it has already exited A and return to no wind.

If the player starts inside a nested volume Verdant will do its best to figure out which volume is the lowest, but this only works if the volume is fully contained by the others. For cases like the previous overlap there is no way to know from where the player might have entered, and the active volume will be chosen at random. Try to avoid spawning the player in intersections. 

While wind is always applied globally there is actually another way to influence vegetation interactively in specific areas. We'll cover how in the next guide on [using affectors](UsingAffectors.html)!