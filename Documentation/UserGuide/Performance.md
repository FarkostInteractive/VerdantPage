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

Most of the techniques you would use to profile Unity normally still apply here. The difference is that Verdant adds a layer of abstraction, in that the actual rendered scene is created dynamically from the parameters on its components and types. Those parameters are where we'll be doing our work. You'll need to find which ones whatever whatever you're looking at, which is usually as easy as finding the VerdantObjects/VerdantTerrains and checking what groups and types they're using. 

The next step is finding your bottleneck. Doing so can be difficult without a full understanding of the systems involved, so in the next sections we'll go through some common scenarios and figure out what to do about them.

### Diagnostic Tools
There are two tools available to help with diagnosing problems. You'll find both of them under Verdant in the menu bar. The first is the [Debug Panel, which has an advanced guide all of its own](). It helps you understand how Verdant is interpreting your scene at any given time and is very good for identifying sources of visual artifacting. There is also Rendering Statistics, which shows all the types in the scene and gives you their instance counts in real time. Use it to identify which types have the highest presence at different points in your scene. 


## Density and vertices

It probably goes without saying that even though Verdant allows you to have a lot of instances, decreasing the amount will always have positive effects on your frame times. What might be less obvious is that your vertices per instance also matters a lot. Verdant essentially renders all of its instances as a single huge mesh and then performs all the per instance work in the vertex shader. In effect, by rendering 10 000 instances with 10 vertices each you are rendering a single mesh with 100 000 vertices. Thinking about it that way makes it clear that rendering 1000 instances with 100 vertices has the same end result. In both cases there is work being performed on 100 000 vertices. One of the columns in the Rendering Statistics window shows the number of vertices per type in real time. This is the number you should pay the most attention to when profiling.

Controlling the number of instances and vertices is the easiest way to keep down the amount of work per frame. The two best tools to do so are falloff and LODs. We'll look at these in a moment, but it's also worth asking yourself if you can just decrease your density flat out. Coverage Modifier on VerdantCamera gives you an easy way to play around with the total density, and you might find that your scenes look basically the same with a 0.7 multiplier applied. There is always a line above which increasing density will give you diminishing returns, though where that is will depend on your type and scene. You should try to find that line and configure your VerdantTypes around it.

When you need to decrease instance count without decreasing density your best bet is to use Falloff on your VerdantTypes. If the Sharp setting is not strong enough, use the Custom mode and start experimenting with the bar graph. Keep the first bar at max, but see how far you can drag down the others. Like with the general density there's no hard and fast best practice, it all depends on the scene and type. So try it out in a few different scenarios and see where that line is for you.

Another way to do it is to change the rendering distance. Render distance increases the size of the area being filled with vegetation, which is a sector of a circle. The total area grows by the radius squared, though most of that area is zones towards the far end which should have low density because of falloff. If you can't use strong falloff then decreasing the render distance to compensate is a must.

LODs give you an easy way to get high visual fidelity while keeping the vertex count low. It's as simple as making a lower tesselation version of your mesh and applying it in the parameter. What might be less obvious is just how far you can push this. If your render distance is high then the vegetation in the last half or so will be tiny on the screen, so for simple plants like grass you might be able to get away with just a single triangle. Have a look at your types and see how far you can push them!

## Overdraw and pixel shading

Though decreasing instances will never hurt you, it isn't necessarily always the main bottleneck. There are many other issues which might be magnified by a high instance count but are fundamentally different and can be solved in different ways. 

One such bottleneck is the fillrate, which is when the hardware can't keep up with the operations per pixel. Dense Verdant fields will often cover a large part of the screen, so if the resolution is high then the number of pixels occupied by vegetation can get quite big. When there's a lot of work being done in the pixel shader that translates into a lot of operations.

The problem can be further exacerbated by overdraw. Overdraw happens when the same pixel is rendered to more than once. The only value that matters for a pixel is the final color of the surface closest to the screen, so when something gets drawn before that it's just wasted work. Because Verdant renders as much as it can in a single draw call you can pretty easily end up in a situation where more instances than one draw to the same pixel. The risk is further increased the more your instances intersect, i.e. the wider they are and the denser they grow, and by the angle of your camera. If the camera is low to the ground the camera will look through a lot more vegetation, which has other vegetation behind it.

The classic way to test if you are fillrate limited is to decrease the resolution. If the performance is largely unaffected then it's probably something else. If your frame rate goes up the lower the resolution goes then you can be pretty confident that it's the pixel shading. Crucially, you can be fillrate limited even if the number of instances and vertices is low. This can happen if your instances are large enough to still take up a lot of screen space and intersect with each other.

We have two strategies for preventing these issues: Preempting overdraw and decreasing pixel shader complexity. For the first one there's a useful trick we can perform. Each LOD renders as a separate draw call, so by adding a LOD for the first falloff step it's possible to force it to render before all the other instances. When rendering the rest that information will be present in the depth buffer, so later instances will know not to draw to those pixels. It essentially becomes a little shield that prevents the overdraw from getting too bad. This works even if you don't set any parameters on the second LOD, so it's worth doing on all your dense types.

There are also a few ways to prevent Verdant from doing work in the pixel shader. The most important one is to use the Shading Level parameter on the VerdantType. By setting Shading Level you can enable and disable different parameters, which you'll see as them being greyed out. Verdant always tries to branch around unused parameters, but depending on your system that may or may not run more efficiently. By using Shading Level those parameters are stripped out entirely, saving precious time in the pixel shader.

Right below the shading level there's another extremely important parameter called Allow Alpha. It is disabled by default, and there's good reason for that. Enabling alpha prevents Verdant from performing early depth testing, which is the mechanism that allows it to skip pixels covered by other geometry. Be really careful to only enable alpha when absolutely necessary, and try not to do so for dense types. If you must, find a way to disable it in a later LOD. The reason to enable it is if it can save you a lot of vertices by using cutouts rather than complex meshes. Finding the balance there will depend on your target system, game and other specific circumstances. 

## Shadows and forward rendering

Some effects in Unity force all the geometry they touch to be rendered again. When drawing large quantities of instances the prospect of drawing it all more than once becomes quite risky. This is the reason I started this guide by strongly recommending you use deferred rendering! Lights in forward rendering are one of the main perpetrators of this problem. Deferred rendering draws everything in the scene once, then passes over the result to add lights and other effects. Forward rendering by comparison draws each mesh once for each light touching it, which progressively adds up to the finished result. When the "mesh" is a field spanning most of the scene then redrawing it for every light it touches becomes a very bad idea.

In deferered this is not a problem at all. If you must use forward lighting, the best way to circumvent it is to put Verdant on its own layer using either the override on VerdantCamera or the VerdantType Layer parameter. You can then use a separate set of lights that only apply to that layer and keep it as minimal as possible.

The other potential cause of re-rendering is shadow casting. When shadow maps are drawn the light renders all the shadowing objects from its perspective. This pass is fundamentally simpler than re-rendering the scene normally, but presents another problem: Verdant culls based on the camera. There is no way for it to cull from the perspective of the light, so it will render what is already in the scene. For directional lights this works just fine, but spot and point lights will inevitably render more than they need to. 

Like with lights in forward rendering you can set up a different set of shadowing lights just for Verdant, but you should also consider whether you need shadow casting at all. Receiving shadows is cheap, and a sufficient amount of other shadowing objects can mask that the vegetation itself is not casting shadows.

You also have the option of using LODs. For many types of vegetation the user won't be able to see shadows after just a step or two of falloff, so there's no reason to keep them enabled. It's good practice to always use at least two LODs for shadowing VerdantTypes and to find the closest possible point where they can be disabled. 

For less dense VerdantTypes re-rendering is a much smaller problem. A good rule of thumb is that unless your type is meant for large fields there's little need to worry about shadows or lighting. Sparse types or spread-out patches will run just fine regardless. It is when you use Verdant to do things that Unity normally wouldn't handle as well, like filling thousands of square meters, that the rules change a bit and you need to be more careful. 


## Using LODs effectively

We've already touched on a few things you can do with LODs, but I want to reiterate that almost all the optimizations mentioned previously can be set per LOD. Adding a LOD is quite cheap (and you might already have two to prevent overdraw!), so ask yourself if there are parameters you don't want to lower on the first LOD but can maybe spare later down the line. The shading level is a great example of this, as many of the advanced effects won't be visible at a distance. The same thing goes for Allow Alpha, where you can often replace an alpha cutout with a mesh in roughly the same shape.

Also remember that LOD fade is very useful for hiding pop in. If it helps you hide disabling shadows or switching to a lower resolution mesh early then don't hesitate to use it. It is there to help! Do keep in mind though that LOD fade won't be very noticable further away from the camera, and that it's performance cost grows with distance (as the falloff steps grow in covered area). Use it as much as you need to, but do make sure to test how much it helps.

## Optimizing subsystems

Beyond what ends up on the screen Verdant always has a fair bit of work going on in the background. It is meant to be unobtrusive, but like everything else it does come at a performance cost. If you find that you have issues unrelated to any of the previous categories, or if you just want to shave off a few extra corners, then this section describes how to cut down on that work.

Most of the background work involves building the field data structures that Verdant relies on to draw vegetation. For all of them there is a tradeoff we can make: Decrease their resolution to decrease the amount of work performed, but lose some of the detail in Verdants' perception of the game world in the process. Depending on the game doing this might lead to visual artifacting or it might be completely fine. It all depends on your needs.  

### VerdantCamera 
VerdantCamera is responsible for managing all the VerdantTypes in the scene and drawing them onto appropriate surfaces. xxxxxxxxxxxxxxxxx

For VerdantCamera, a decrease in resolution means xxxxxxx

### Affectors

Adding an affector field to your scene is essentially like adding a new feature into Verdant. A VerdantCamera without a field component won't be doing any of the work related to it, so if you just want to keep everything as lean as possible it is best to not use fields at all. When you do need them though there are steps you can take to reduce that cost.

As with VerdantCamera you can change the range and the resolution. These two together determine both tthe granularity of the field and its performance cost. Low resolution is cheaper and the range controls how large the area we stretch the resolution over is. 

xxxxxxxxxxxxxxxxxxxxxxx

* The number of affectors matters less than having a field in the first place
* Resolution helps, especially for deflection
* As does the updates per second
* Use simple meshes


## Sending you off

That's it! Congratulations on making it all the way through, you now know all you need to start exploring Verdant on your own. When you have questions further onward the two best places to go will be the [Advanced Guide](AdvancedGuide) which walks you through some of the more specific subsystems, and the [Component Reference](ComponentReference) which contains descriptions of basically every component and parameter in Verdant. There is also the [FAQ](FAQ), which aims to catch anything that might otherwise slip through the cracks. 