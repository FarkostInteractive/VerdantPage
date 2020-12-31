---
layout: default
title: "Workflow"
parent: "User Guide"
nav_order: "1"
---

# Workflow

This guide takes you through the process of building a scene with Verdant. 

## Surfaces and zones
As previously mentioned, the main usage of Verdant is defining zones for vegetation to grow in. In Getting Started we did this by adding the component VerdantObject to a box, which made grass appear on top of it. In the background adding VerdantObject made Verdant register the mesh of the box as a surface tracked within its systems. By then adding a type that surface was defined as a zone where VerdantNormalUpGrass grows. 

![Zone surface image]()

In short, a mesh recognized by Verdant is a surface, and on top of a surface we can have a zone of one or more types of vegetation. 

If you are used to thinking of vegetation as instances you paint onto geometry it's worth really internalizing this model. Think of VerdantObject as draping a piece of cloth over the mesh. The 'cloth', or the Verdant surface, is the only thing intrinsic to that object. It is not until the surface is observed by a camera that anything actually gets placed on it. 

Shaping these zones and surfaces is what we'll be using everything in this guide for. It's a very different model, but one that lets us do a number of interesting things. Let's have a look at some of them!

## Masking

Most scenes are more complex than a box with some grass on it. Even for scenes where that is enough we might want to make the grass a bit patchy or add some nice scale variance. We can use a number of different masking techniques to achieve this.

![Masked box]()

The simplest way to add a mask is to use the Scale Mask parameter on VerdantObject. You'll find it under the Masking Texture dropdown. This parameter takes a greyscale texture and uses it to scale the vegetation on the object. Black being zero and white being one, the value is multiplied into the scale set by the Scale parameter. 

The other parameters can be used to adjust the mask in a number of ways. You'll find in depth explanations in the VerdantObject component reference, but for now it's worth pointing out Min Scale and Max Scale. These let you change what values black and white map onto. For example, setting Min Scale to 0.4 makes that the new lowest value, so the texture will range from 0.4 to 1.0. Changing them can have quite significant effects on the character of your mask. 

![Gentler masked box]()

### Mask objects

Scale masks are well and good for simple uses, but there are more powerful tools in our belt. If the ground consists of several different objects and we want a consistent pattern or shape across all of them assigning individual scale masks become very cumbersome. 

![Mask object]()

It is better to use a single mask object. We can create these from a VerdantObject by changing its Mode parameter from Surface to Mask. In Mask mode the object can only be used to create zones on other surfaces, as the object itself is no longer a surface. Think of it as projecting the mesh downward onto the VerdantObjects below.

![Mask object w texture]()

The simplest usage is to just project the shape of the mesh, but we can also add a scale mask texture onto it. The texture will be projected along with the mesh, effectively giving us a way to apply a scale mask to several objects at once.

All the same parameters apply as on a regular scale mask, but there is one unique to mask objects worth pointing out. Setting Type Only makes it so the mask won't scale vegetation below it. Instead, it will simply add its types into it. It will do this anywhere its scale texture is not zero. This enables masks to add vegetation in shapes defined by scale masks without affecting what is already there.

![Type only mask]()

### Painted masks

If a normal scale mask isn't working for you Verdant includes tools for painting a mask yourself. These tools are more powerful in that they also allow you to paint different types directly into the mask. There is a full [Guide to painted masks]() in the advanced guide section. Just like scale masks painted masks can be used on VerdantObjects in both modes.

## Terrains

VerdantObject is specifically meant to be used for meshes, but it is not the only way to create surfaces. You can also use VerdantTerrain to create them from terrains. These work mostly the same as VerdantObject, with the exception that you can link types to different terrain layers. This allows you to, for example, easily add grass onto a grass texture you've already painted in. Just like VerdantObject you can always project a mask onto a terrain, even if it doesn't have types of its own.

