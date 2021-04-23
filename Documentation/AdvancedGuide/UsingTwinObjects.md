---
layout: default
title: "Using Twin Objects"
parent: "Advanced Guide"
---

# Using Twin Objects
Twin objects are a useful way to take Verdant instances and dynamically transform them into full gameObjects. By using them you can create the illusion of a much higher level of interaction than Verdant can normally support.

[]

The basic principle is to replace Verdant instances with gameObjects as the player approaches them. Verdant will spawn an instance of the prefab and in the same moment hide its own instance. When the player moves out of range the twin is disabled and added back into a pool of twins to be reused when needed, and the GPU instance is shown again. If configured correctly the effect is seamless, so the two appear to the player to be one and the same.

## Testing
An important thing to know is that twin objects are intentionally disabled in edit mode. Unless you've customized the editor there's nothing there to interact with them, and most importantly it prevents Verdant from spawning unwanted objects that might accidentally be saved or fill up the hierarchy. When everything else renders and animates in the scene view it can be easy to forget that, so take care to always run the game in play mode when testing your changes.

## Setting up a type
VerdantType has a parameter towards the very bottom called Twin Object. It takes a prefab, and when set Verdant will automatically start to replace instances of the type with it at runtime. The field accepts any prefab, but will throw an error at runtime unless it has a component inheriting from [VerdantGameObjectTwin](../ComponentReference/VerdantGameObjectTwin.html). 

## The twin object prefab
[VerdantGameObjectTwin](../ComponentReference/VerdantGameObjectTwin.html) is an abstract component that has four methods you need to implement to help manage the object over its lifecycle. As the camera moves around twins are added into and taken out of the pool constantly. It's important that the object knows how to respond to these events in a performant way, and that it knows to reset itself so it can be reused correctly. Please follow the link above to the component reference for more details about the methods.

If you're using twin objects as a way to animate complex vegetation physically, eg. rigging it up with joints, rigidbodies and colliders, then there's an included implementation of VerdantGameObjectTwin called VerdantPhysicalTwin that you can use. Add it to the root object and it should take care of the rest. Note that objects like this are quite expensive to reset, so if you have a more narrow use case that you could optimize for it's very worth doing so.

## Visuals
Unless the twin prefab looks exactly the same as the VerdantType, replacing one with the other is not going to look right. If you haven't overridden it you should use the Verdant Standard Shader on all the prefab materials and set the parameters as they are on the VerdantType. You should also [enable global parameters](AccessingVerdantData.html) on any relevant [VerdantCamera](../ComponentReference/VerdantCamera.html) parameters and on your [affector fields](../ComponentReference/Fields). That way the material will pick up on their contents and match how the GPU instances look.

For the same reasons, make sure all the scales and rotations are the same. There shouldn't be any scaling or rotation applied on any of the prefab transforms unless it matches the mesh set on the VerdantType.

## Replacement Threshold and Retrieval Range

With the prefab set and the component implemented you should be able to run the game and see your instances be replaced. To make this clearer it can be useful to temporarily set a different color or some other conspicuous difference on the twin materials.  

There are two more parameters we need to look at: Replacement Threshold and Retrieval Range. These rather confusingly named parameters control how and when instances are replaced. To understand them it helps to know a bit about how the system works in the background. Verdant will read back the positions closest to the camera from the GPU and store them CPU-side. It then keeps track of these positions and places twins on them when the camera is close enough. "Close enough" here is the replacement threshold. So basically, retrieval range controls the area that Verdant is aware of, and replacement threshold is the distance at which a GPU instance becomes a prefab instance.

Normally you can just set Retrieval Range to about double Replacement Threshold. That gives plenty of margin while not forcing Verdant to read too much at once. If the camera moves fast increasing Retrieval Range can help make reads less frequent. If the CPU ends up chugging on all the positions or you have hitches from reading back the positions then decrease it instead.

Also note that twin objects always assume full density, so the replacement threshold should be within the falloff steps where density is at maximum. If there is falloff applied Verdant will try to replace instances that are no longer there. If you need a threshold bigger than the first falloff step then increase it so you have more than one step at maximum.

## Performance

In terms of just management twin objects are fast enough to support big areas with rather dense types. They are meant to be used with bigger plants like ferns and bushes, but you don't necessarily need to have them all spread out. Most of the performance cost actually comes from the gameObjects themselves. Unity doesn't handle large numbers of gameObjects all that well and some components can become very expensive at scale. Enabling and disabling a lot of objects can also be heavy. which is why it's important to implement [VerdantGameObjectTwin](../ComponentReference/VerdantGameObjectTwin.html) correctly. If the type is dense and the player moves fast then it's likely that dozens of objects are brought in and out of the pool per frame. In general you should only reset what you strictly have to. The less state each instance has the better.

