---
layout: default
title: "Performance"
parent: "User Guide"
nav_order: "6"
---

# Performance
{: .no_toc }

This might not be the most exciting guide in the world, but it is nonetheless very important. By now you've seen the ways in which Verdant is different from other vegetation systems, and now we'll look at the set of performance quirks that come along with that. Being aware of them will help you avoid common pitfalls and make for a much smoother experience in the long run.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## How to think about performance

Verdant exists to solve a very specific problem, and that is to make creating large, dynamic worlds with dense vegetation as seamless and quick as possible. That is why it works the way it does, creating instances and interpreting the world as it goes along rather than baking everything into huge scene files beforehand. There are many natural strengths to this approach, chief among them completely uncoupling the size from the world from how your game performs and keeping a small and constant memory footprint. 

But it does also mean that we need to focus on the parameters that create the scene rather than the scene itself when optimizing. If the framerate stoops when you look at a specific field, the culprit is going to be either the VerdantGroup it's sourced from, its constituent VerdantTypes or the VerdantObject on which they grow. Once we know where the problem is we need to identify what type of problem it is, and then know what parameters to change to reduce it. In the next sections we'll go through the broad categories of performance issues, what they arise from and ways to cover them up. 

### Diagnostic Tools
There are two tools you can use to help with diagnosing problems. You'll find both of them under Verdant in the menu bar. The first is the [Debug Panel, which has an advanced guide all of its own](). It helps you understand how Verdant is interpreting your scene at any given time and is very good for identifying sources of visual artifacting or why affectors are acting up. There is also Rendering Statistics, which shows all the types in the scene and gives you their instance counts in real time. Use it to identify which types have the highest presence at different points in your scene. 

## Density and vertices

While instances are cheap in Verdant each one does still come with a cost. Verdant does all it can to cull their numbers, but you could still find yourself rendering more instances than necessary. 

The easiest way to test if density is causing you problems is to use Coverage Modifier on the VerdantCamera. 

In the last guide we went over some of the ways to make up for a lower density.

## Pixel shading and overdraw

Depending on the mesh you're using 

## Shadows and forward rendering
One thing that becomes very dangerous when rendering all these instances is making the engine render it more than once. 

## Using LODs effectively

## Optimizing subsystems
If the problem isn't any of the above there's a decent chance it is actually one of the systems running in the background. These can be a little opaque, but there's still a lot you can do to influence them.

### Affectors

### Height and Typefields

### Wind

## Limiting the number of instances
As shown in the last guide, there comes a point where raising density doesn't actually do much for your visuals anymore. Try to find where that line is for you, a very easy way to test it is using the Coverage Modifier on your VerdantCamera. While instances are cheap they are not free. If you can go from density 100 to density 75 that can save you something like 50000 instances when looking into a large field, which is GPU time that you could be spent elsewhere. You should also consider hooking up the Coverage Modifier to your graphics settings via script. That is the easiest way to make Verdant run better on low-end systems.

Just as important as the instance count is the number of vertices per mesh. The statistics view shows you the total number of vertices next to the number of instances, and this is really the number you should be focusing on. There's not much difference for Verdant between ten meshes with 100 vertices and 100 instances with ten vertices, their cost will be roughly the same. We'll look at how to get around this with LODs in a moment, but for now keep in mind that your meshes should have as low vertex counts as you can get away with.

You can also try lowering the render distance and experiment with falloff. You'll be surprised by how low you can set the furthest falloff steps without really noticing a difference. If the camera is looking along the ground all the instances in front of it accumulate, so you'll barely even see the ones furthest away. Because the view frustum grows outwards with distance each falloff step is significantly larger in area than the previous. That further increases the potential gains of a sharp falloff, with no falloff the amount of instances in the last steps would be enormous. 

## Lighting
Verdant supports both forward and deferred lighting. In general it follows the same principles as everything else: Deferred will perform better when you need a lot of lights but is less flexible and has a higher base cost. In the majority of cases Verdant will perform best with deferred (for which you'll need to set up its custom deferred shader as covered in Getting Started), so you should use it when you can. 

Remember that you can set layers both on types and on the camera itself. You can use this to cull some of your more expensive lights or to set up a specific light rig just for vegetation. 

## Shadows
Of all the parameters in VerdantType there is nothing more treacherous than enabling shadow casting. It can completely transform the look of your scene and give it a gorgeous, lush sense of volume, but it will also force Verdant to re-render everything from the perspective of each shadowing light. Verdant has no way to cull from the perspective of a light, so that means redrawing every instance that has shadows enabled, even ones that seem unaffected. You should almost always restrict the amount of shadowed instances by using LODs and avoid having more than one shadowing light. This is another case where using layers to cull lights can be very useful. Think about if you need shadows at all, in well lit scenes with a sun high in the sky they might not be very noticeable. 

If your vegetation isn't very dense you don't need to worry as much about shadows. Scenes that are already cheap and have a low number of instances shadows will see a much lower impact from enabling them, whereas wide dense fields warrant the most caution. 

It's also worth noting that receiving shadows is a completely different thing from casting them. Receiving shadows is essentially free in deferred rendering and quite cheap in forward, so you rarely need to worry about it at all.

## Shading levels and alpha
Like the Unity Standard Shader the Verdant shader is built to be flexible and support a range of visual styles. That does mean that it might be doing more work than you need it to sometimes, so you can force it to exclude certain features by setting the Shading Level on your VerdantTypes. When you change it you'll notice that some texture parameters become greyed out. Verdant always tries to branch around unused textures, but how much time that saves you depends on your systems graphics architecture. Using the shading level guarantees that the greyed out textures will be passed over completely. This is a per LOD parameter, so it's possible to have a higher level for your first LOD than the latter.

Even more important than the shader level is Allow ALpha, which enables and disables alpha clipping. Alpha clipping can be a very useful way to keep your vertex count down but has the unfortunate side effect of preventing the GPU from doing early Z tests, meaning it will draw many more pixels than needed. This is especially pronounced in dense fields, where the grass closest to the camera might occlude thousands of other instances. In scenes with a lot of overdraw you should always try to keep this option disabled, though you might have to weight it against the cost of having more complex meshes.

If you know how to do it and have a very narrow visual style you can gain a lot of performance by writing your own shader. Have a look at the advanced guide for [writing custom shaders]() if you're interested in doing so.

## LOD usage
Many of the previously mentioned problems are per LOD adjustable, meaning we can reduce their impact by only allowing them on the first LOD. In games with high visual fidelity you should almost always have at least two LODs, one with all your high detail effects and one that has been reduced as much as you can get away with.

The most obvious case is using more than one mesh. A mesh with a slightly higher vertex count can look better when animated, but as explained we don't want it multiplied out into every instance in the scene. By having a version with high tesselation and one with low you get the benefit of both. The same thing goes for shadows, shading level and even alpha clipping. 

Even if you don't need LODs there's actually an argument for adding one just for the first falloff step anyway. As mentioned, dense vegetation can lead to a lot of overdraw. When you use LODs Verdant is forced to draw each one as a separate draw call, and it will draw them front to back. By drawing the zone closest to the camera first it will almost certainly block out a lot of the screen. That information will then be present in the depth buffer when the next LOD is drawn and it can use it to avoid drawing to those pixels again. You'll see the most benefit from doing this if you have large dense fields, less so if your scene mostly consists of small patches. 


## Field and affector performance
When you add a field component you are essentially including a new feature into Verdant, and that has an associated cost. All fields need some memory to keep track of their affectors, and deflection needs to run its simulation at the set interval. Instances will also need to read from each field to apply their effects, increasing the cost per instance slightly. The most dramatic cost comes from adding the field at all, though there are other factors as well.

The resolution of the field, which is closely associated with its range, should be kept as low as possible.  

In general, affector performance is less about total number and more about density. You can easily have hundreds of affectors in your scene if only three are in range at any given time. This is why it's important to be mindful of the range and resolution of your fields. 
