---
layout: default
title: "Visual Flair"
parent: "User Guide"
nav_order: "5"
---

# Visual Flair
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

With all of its many, many parameters it can be tempting to just turn everything in Verdant up to max and get as much visual juice out as you possibly can. More likely than not you'll just end up with some fancy mush and a very unhappy computer. By working a bit more thoughtfully we can combine different features to reinforce each other and get scenes that are both very pretty and performant. In this guide we'll cover the most important parameters and systems, looking at how they play into different scenarios and how they all fit together.

Like the last part in Workflow this guide is structured as a series of steps. We'll be relying on everything covered so far, but the step by step bits in Getting Started and Workflow are especially important if you want to follow along. Make sure you have your scene set up right and are comfortable with using VerdantObjects and VerdantTerrains. 

The next guide is a companion to this one and covers the performance impacts of all these features. It is very important! Don't skip it just because you get results you like after reading this one! Knowing what not to do will help you in every decision you make and save you a lot of time as your game grows.

## Building the scene

I've prepared my scene with a simple terrain. We'll also be using a basic VerdantType which has been set up as described in Types and Groups. By starting from something simple we can build it up as we go. Here is my scene with a VerdantTerrain using the type:

Not very interesting as of yet, but it'll get there! 

We should take a moment to think about what we even want from a lush grassy field. What is it that makes this one lackluster? One thing that immediately sticks out is that all the instances blend sort of blend together. 

In short, a good field should:
* Look smooth and natural but have enough variation to show that it's made up of separate instances
* Look dense enough that the ground underneath it seems to disappear
* Interact with the light in interesting ways
* Grow in interesting shapes
* Each individual plant should look interesting, even up close
* There should be more than one type growing together

## Variation

## Density

## Lighting
* Skybox and baked light

## Interesting Masking

## Adding another type

## Density
It can be tempting to raise your instance count as high as possible, but beyond a certain point the returns tend to diminish. Where that point is depends on the type of game you’re making, the Verdant Type you’re using and where the camera is placed. Only you can decide what is right for your project, but remember that your perspective can easily become skewed from repeated exposure. Chances are your audience won’t actually notice whether you use 100 or 125 instances per square meter. 

I recommend messing with the Coverage Modifier parameter on your Verdant Camera often, because you might be surprised by how low you can set it without losing your visual identity. As your scene grows more complex, maybe the audiences eyes are drawn more to other elements in the scene? Even going down to 75% of your original count can have a big performance impact!

You can also set the Coverage Modifier from script at runtime based on different graphics settings or build platforms. Have a look at the scripting reference for more details.

## Lighting

Depending on the type of vegetation 

In general, the more your mesh is like a billboard the more you want to use the normal up mode. It excels when the individual instance matters less than the field as a whole, and it will light the field as if it was part of the ground it is placed on. For thick vegetation like grass this is almost always what you want.  

On the other hand, when your mesh is a detailed fern, bush or other type of shrubbery you might want to use the translucent mode. This mode is more like the standard Unity shader, but also enables the object to be lit both from the front and back.

## Translucency

Translucency means slightly different things depending on the light mode you’re using. In the translucent lighting mode it is very literal. How much light does this leaf let through to the other side?

[Image]

The normal up mode interprets translucency slightly differently. You can think of it almost as specular lighting, but limited to the translucency texture. This allows you to create beautiful sun glitter, akin to a sunset over the sea. Depending on the lighting and type configuration this effect can be either subtle or very dramatic. 

## Base Textures and Size Masks

Adding variance is maybe the most important thing . The number of instances in use doesn’t matter much if they all blend together.

Vegetation is rarely completely uniform in real life, so adding some scale variance should be one of the first things you do. 

Verdant Objects allow you to add a Scale Texture.

You can also use a Verdant Object in Mask Mode. This way we can add scaling to  





## Multiple types

Variance can also come from using multiple similar meshes. Verdant includes a feature called a “Type Group” to make it easier to use several different types together. 

A Type Group is easy to set up. Just create a new Type Group Asset in the Project View and assign your types to the group list in the inspector. The instance count multiplier on the right functions like the coverage multiplier on the camera. It makes it easy to split three different types into say, 15%, 15% and 70%. Adding multiple types to a group without accounting for their total coverage can be dangerous. If each type has a coverage of 50 instances per square meter three of them together will be 150 instances per square meters, and three times as expensive to draw. Make sure you split them up or account for their combined usage on the type itself. 

Once set up the group is for all intents and purposes just another type and you can assign it to any object you like.

Type Groups are generally good for performance, because a group will be treated as a single type by the culling system. This decreases overhead. Do note however that to split the types in a group you will have to use the split type by itself. If grass, flowers and wheat are grouped and you also want to use flowers separately the separated flowers will be counted as a second type. 

Also note that you can use a maximum of 31 separate types in any given scene. Grouped types get around this limit, as each group can contain a virtually unlimited number of types.
 
## Shadow Casting

Shadow casting i. When every blade of grass casts a shadow onto the others it can create a beautiful, complex voluminous effect. It shouldn’t come as a surprise then that it is also the most expenseíve visual element to enable. As in any other context, adding shadows requires redrawing the entire model from the perspective of the shadowing light. Just a single light can make your scene twice as expensive to render.

You should think carefully about whether you need shadows. If the sun is high and your ambient lighting strong chances are they’ll barely be visible and not worth the cost. But if the sun is low and you’re aiming for that dramatic effect there are some tricks you can use to get around the costs.

There are several ways to fake shadows:
* A sharp translucency texture can make it appear as though only the top of a blade of grass is lit
* Using ground normals (which aligns the grass normal to the ground below) will incorporate some of the diffuse lighting from the ground and make broad shadowing unecessary. You can set ground normals higher than 1 to make this effect more dramatic. 
* A base color texture with a cellular noise can add a similar kind of complexity to the field.
* Cloud shadows can take some of the players attention away from shadowing between vegetation. So can other features in the scene that cast detailed shadows or draw attention.
* Use the LOD system to disable shadows on all but the first LOD. This can make a huge difference and combined with the other techniques get you very close to the full shadowing look.

You should also be mindful to not have more than a single shadowing light. Verdant lets you set which layer to draw to both on the type and the camera, which you can use to cull your lights if you need shadows for other objects. 

It should be noted that receiving shadows is different from casting shadows. The performance cost is negligible in forward rendering, and in deferred rendering it is handled as part of the main shading step and can’t be disabled. You can safely leave it on in most cases.

## LODs

The page on Verdant Types goes through all the different parameters 

## Chosing a Ground Texture

You can also tap into Verdant directly and access the scale, wind and affector fields 
