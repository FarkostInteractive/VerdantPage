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

We've already touched on a few things you can do with LODs, but it's worth reiterating that almost any optimization mentioned previously can be set per LOD. Adding a LOD is quite cheap (and you might already have two to prevent overflow!), so consider that there might be parameters you don't to lower on the first LOD but can spare later down the line. The shading level is a great example, as many of its effects won't be visible at a distance.



* It is always worth adding an extra LOD if it lets you simplify something
* Remember that you can drop shading level and allow alpha
* LOD fade is good but don't overdo it

## Optimizing subsystems

* If you must, decrease resolution

### Affectors

* Use simple meshes




























## How to think about performance

Verdant exists to solve a very specific problem, which is to make creating large, dynamic worlds with dense vegetation as seamless and quick as possible. That is why it works the way it does, creating instances and interpreting the world as it goes along rather than baking everything into huge scene files beforehand. There are many natural strengths to this approach, chief among them completely uncoupling the size from the world from how your game performs and keeping a small and constant memory footprint. 

But it does mean that we need to shift our focus when optimizing from the scene itself to the parameters that create it. If the framerate stoops when you look at a specific field the culprit is going to be either the VerdantGroup it's sourced from, its constituent VerdantTypes or the VerdantObject on which they grow. Once we know where the problem is we need to identify what type of problem it is, and then know what parameters to change to reduce it. In the next sections we'll go through the broad categories of performance issues, what they arise from and ways to reduce their impact. 

### Diagnostic Tools
There are two tools you can use to help with diagnosing problems. You'll find both of them under Verdant in the menu bar. The first is the [Debug Panel, which has an advanced guide all of its own](). It helps you understand how Verdant is interpreting your scene at any given time and is very good for identifying sources of visual artifacting. There is also Rendering Statistics, which shows all the types in the scene and gives you their instance counts in real time. Use it to identify which types have the highest presence at different points in your scene. 

## Density and vertices

* Don't have more instances than necessary
* Use falloff
* Consider your vertices
* Check the stat tool

While instances are cheap in Verdant each one does still come with a cost. Verdant does all it can to cull their numbers, but you could still find yourself rendering more instances than necessary. 

The easiest way to test if density is causing you problems is to use Coverage Modifier on the VerdantCamera. 

In the last guide we went over some of the ways to make up for a lower density.

## Overdraw and pixel shading

* Don't have your instances too dense
* Don't have your instances too big
* Disable Allow Alpha
* Keep the pixel shader simple by using few textures, Allow Alpha and Shading Level

After the instance count the next step is to check how much your instances overlap. Some amount of overlap is a good thing and fundamentally the reason to increase density in the first place. The lusher and denser fields are the nicer they look, right? The unfortunate side effect is that because Verdant renders as much as it can in a single draw call its capacity to Z-test against itself is limited. It can be very easy to end up with a lot of overdraw. 

Overdraw happens when a pixel is rendered to several times. We're only interested in the final color drawn by the surface closest to the screen, so every time another surface covering the pixel is drawn beforehand there's redundant work being performed. The classic way to test for overdraw is to isolate the problem, then decrease the resolution and see how much your framerate improves. If Verdant renders significantly faster at lower resolutions chances are the amount of overdraw is limiting you more than your instance count. For this reason you need to be especially mindful of overdraw when targeting systems with high DPI screens. 

As the problem is still fundamentally tied to density decreasing the number of instances will still help you here. The nuances are slightly different with overdraw though. You'll specifically want to strengthen falloff, as overdraw happens most when the camera is close to the ground and looking at vegetation head-on. By decreasing the density in the areas that are already occluded you can both save instances and prevent overdraw.

You also need to consider the scale and extent of each instance. If the density is low but the meshes are wide enough to still intersect the effect is essentially the same as with higher density. For this reason it can be possible to have a surprisingly high instance count when using narrow, simple meshes.

The strongest weapon you have is the VerdantType parameter Allow Alpha. By flat out disabling any sort of transparency the shader becomes able to use early Z-testing and save some work even within a single draw call. You should always disable alpha when you can, as it can often double your framerate. The tradeoff here is between using alpha or increasing the number of vertices to model the shape of your plant. Which is the better choice will depend on your circumstances.

We can also use LODs to give the depth buffer a helping hand. By creating a LOD that covers just the first falloff step we force Verdant to render it first. All the closest instances will be present in the depth buffer already when the rest are rendered, meaning they can be reliably depth tested against. As the closest instances are most likely to cover up other instances this first pass will protect against the worst overdraw scenarios. To some extent this approach extends to making LODs for later steps too, though your gains might be overtaken by overhead at some point.

A related problem is the complexity of the pixel shader. If the pixel shader is performing a lot of work then overdraw becomes an exponentially bigger problem, and scenes with little overdraw can become expensive anyway when Verdant covers a lot of the screen.

You have three tools at your disposal to keep the pixel shader lean. The first is the aforementioned alpha switch, which will cut some work in addition to allowing early Z-testing. The second is the similar Shading Level parameter. Shading Level allows you to control which features you want the shader to include, you'll see which are contained in each by the parameters that grey in or out. The Basic level is a bare minimum, allowing you a texture and little else. Verdant Standard is intended as the main level and allows all the features you need for rich and lush vegetation. Full Surface Detail allows you access to all the same parameters as the Unity standard shader. 

The third tool is to simply not use some of the texture parameters. Verdant will try to branch on these to avoid doing unnecessary work if they aren't set. How effective that is will depend on your system architecture though, so to guarantee that a parameter isn't used you'll need to use Shading Level to deactivate it.

All of the mentioned techniques are LOD-adjustible, so you can potentially save a lot by dropping down a shading level or two in a later LOD. 

## Using LODs effectively

* It is always worth adding an extra LOD if it lets you simplify something
* Remember that you can drop shading level and allow alpha
* LOD fade is good but don't overdo it

## Optimizing subsystems

* If you must, decrease resolution

If the problem isn't any of the above there's a decent chance it is actually one of the systems running in the background. These can be a little opaque, but there's still a lot you can do to influence them.

### Affectors

* Use simple meshes

## Limiting the number of instances
As shown in the last guide, there comes a point where raising density doesn't actually do much for your visuals anymore. Try to find where that line is for you, a very easy way to test it is using the Coverage Modifier on your VerdantCamera. While instances are cheap they are not free. If you can go from density 100 to density 75 that can save you something like 50000 instances when looking into a large field, which is GPU time that you could be spent elsewhere. You should also consider hooking up the Coverage Modifier to your graphics settings via script. That is the easiest way to make Verdant run better on low-end systems.

Just as important as the instance count is the number of vertices per mesh. The statistics view shows you the total number of vertices next to the number of instances, and this is really the number you should be focusing on. There's not much difference for Verdant between ten meshes with 100 vertices and 100 instances with ten vertices, their cost will be roughly the same. We'll look at how to get around this with LODs in a moment, but for now keep in mind that your meshes should have as low vertex counts as you can get away with.

You can also try lowering the render distance and experiment with falloff. You'll be surprised by how low you can set the furthest falloff steps without really noticing a difference. If the camera is looking along the ground all the instances in front of it accumulate, so you'll barely even see the ones furthest away. Because the view frustum grows outwards with distance each falloff step is significantly larger in area than the previous. That further increases the potential gains of a sharp falloff, with no falloff the amount of instances in the last steps would be enormous. 

## Shading levels and alpha
Like the Unity Standard Shader the Verdant shader is built to be flexible and support a range of visual styles. That does mean that it might be doing more work than you need it to sometimes, so you can force it to exclude certain features by setting the Shading Level on your VerdantTypes. When you change it you'll notice that some texture parameters become greyed out. Verdant always tries to branch around unused textures, but how much time that saves you depends on your systems graphics architecture. Using the shading level guarantees that the greyed out textures will be passed over completely. This is a per LOD parameter, so it's possible to have a higher level for your first LOD than the latter.

Even more important than the shader level is Allow ALpha, which enables and disables alpha clipping. Alpha clipping can be a very useful way to keep your vertex count down but has the unfortunate side effect of preventing the GPU from doing early Z tests, meaning it will draw many more pixels than needed. This is especially pronounced in dense fields, where the grass closest to the camera might occlude thousands of other instances. In scenes with a lot of overdraw you should always try to keep this option disabled, though you might have to weight it against the cost of having more complex meshes.

## LOD usage
Many of the previously mentioned problems are per LOD adjustable, meaning we can reduce their impact by only allowing them on the first LOD. In games with high visual fidelity you should almost always have at least two LODs, one with all your high detail effects and one that has been reduced as much as you can get away with.

The most obvious case is using more than one mesh. A mesh with a slightly higher vertex count can look better when animated, but as explained we don't want it multiplied out into every instance in the scene. By having a version with high tesselation and one with low you get the benefit of both. The same thing goes for shadows, shading level and even alpha clipping. 

Even if you don't need LODs there's actually an argument for adding one just for the first falloff step anyway. As mentioned, dense vegetation can lead to a lot of overdraw. When you use LODs Verdant is forced to draw each one as a separate draw call, and it will draw them front to back. By drawing the zone closest to the camera first it will almost certainly block out a lot of the screen. That information will then be present in the depth buffer when the next LOD is drawn and it can use it to avoid drawing to those pixels again. You'll see the most benefit from doing this if you have large dense fields, less so if your scene mostly consists of small patches. 


## Field and affector performance
When you add a field component you are essentially including a new feature into Verdant, and that has an associated cost. All fields need some memory to keep track of their affectors, and deflection needs to run its simulation at the set interval. Instances will also need to read from each field to apply their effects, increasing the cost per instance slightly. The most dramatic cost comes from adding the field at all, though there are other factors as well.

The resolution of the field, which is closely associated with its range, should be kept as low as possible.  

In general, affector performance is less about total number and more about density. You can easily have hundreds of affectors in your scene if only three are in range at any given time. This is why it's important to be mindful of the range and resolution of your fields. 
