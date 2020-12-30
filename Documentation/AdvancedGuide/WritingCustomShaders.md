---
layout: default
title: "Writing Custom Shaders"
parent: "Advanced Guide"
nav_order: "5"
---

# Writing Custom Shaders

The default Verdant shader is optimized for flexibility and to blend in with the Unity Standard Shader. But there are plenty of cases where you might want to optimize for something else. The default shader is a surface shader, which makes it very comprehensive but slightly inefficient. By building your own shader you can strip out all the cruft and focus on the features you need. It also lets you change the light model if that is something your art style demands. Verdant is built to be easy to extend, and this page will take you through the process of getting everything set up. 

Verdant includes two minimal example files for a surface shader and a simple unlit forward shader. These can be useful as references or starting points, especially if you’re not super comfortable with shader programming. You can find them under ********.

## Preparation
All Verdant shaders have a certain amount of boilerplate for generating shader variants and setting up instancing that we need to include at the start. Copy the block below and insert it after CGPROGRAM.

You also need to include the Verdant library. This can be a bit tricky since Verdant is distributed as a package. 

Finally, add the line below to link up the Verdant Instancing Setup function:

If you need to do some setup of your own you can also call the function directly as in the example below:

## Implementation

You should now be able to set your shader as a shader override on a Verdant Type and see it in the game world, but there’s a bit more work to do if we want to add wind, affector interactions and lighting.  

In your vertex shader, add a call to ******* and pass it your vertex position in object space and normal in world space. This will apply all the various animation and interaction features in Verdant and some per instance randomization. 

If you only want some of these features, I recommend diving directly into the source code. You can copy the ***** function into your shader and strip out the features you don’t want, or pick through the various functions it uses and select the ones you need. 

Finally, we need to add support for dithering in the pixel shader. 

It can also be useful to have a look at the Verdant light model. It has its own .cginclude file at ****/VerdantLightModels.cginc and is based on the Unity Standard shader. You can use either light model in a surface shader as follows:

If you followed all the steps you now have a shader equivalent to the Verdant Standard Shader which you can modify as you please!

## Can I use my own custom deferred shader with Verdant?
There’s no reason you couldn’t! If you don’t need translucency Verdant will probably look fine as is. If you do want translucency you’ll need to add a branching condition that accounts for it. You can either use the code from the Verdant deferred shader or use the data from Verdant and write your own light model. 

**********
* The normal .a channel indicates if something is Verdant vegetation
* If a pixel belongs to Verdant, the occlusion value (diffuse .a) controls translucency
**********