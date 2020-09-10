---
layout: default
title: "Central Concepts"
nav_order: "0"
has_children: true
parent: "Documentation"
---

You might want to think of it similarly to working with particle systems: Most of the time you are not interested in the individual particles. Rather, you are working to shape the effect as a whole by manipulating its parameters.

Working this way enables Verdant to do a lot of powerful things: You can change global vegetation density at any time depending on your performance needs, create fields procedurally using prefabs with built-in behaviour and iterate quickly by changing the types in your scene with just a few clicks. Most importantly, Verdant is completely agnostic to how big your world is. Nothing is ever loaded from disk or swapped in and out of RAM. There’s just one continuous process, constantly filling the world around you with greenery.

## VerdantCamera

The VerdantCamera is where everything starts! By setting one of these on your camera, you tell Verdant that you want vegetation to be placed around it. Note the phrasing here: Vegetation exists in relation to the camera, not the world. If there’s no camera, there’s no vegetation. Just a bunch of objects specifying where vegetation could potentially exist. This also means that there’s no vegetation where there’s no camera, and that wherever the camera moves, vegetation will be placed. 

[Gif showing tiles and frustum]

The VerdantCamera takes the area within your frustum and populates it every frame. 

## Fields

Fields are simplified representations of the scene and the instrument by which the camera knows where and how to place vegetation.

* Determining where to place vegetation instances
* Bending vegetation around GameObjects
* Coloring vegetation

The easiest way to understand 

Fields always exist around the camera. As the camera moves the field moves with it and 

The most important field is the world field, which 

## Objects

So how do we decide what gets to be in a field? There are several ways, and we will start by looking at the simplest one. VerdantObject is a component that 

## Affectors

## Types
