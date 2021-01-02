---
layout: default
title: "Using Affectors"
parent: "User Guide"
nav_order: "4"
---

# Using Affectors

So far we've added a bunch of vegetation and animated it with wind, but we haven't really been able to interact with it at all. Verdant allows us to do that by using affectors, which are components made specifically to influence vegetation at runtime in a few different ways. We can use them to animate grass as a character moves through it, cut it with a sword, rejuvenate it as magic passes through it or any number of other things.

## The three types

Affectors come in three different varieties: VerdantDeflectionAffector, VerdantColorAffector and VerdantScaleAffector. Deflection is for pushing and pressing vegetation as things pass through it, be they vehicles, characters, giant balls or magic spells. Color allows dynamically multiplying new colors into it, either in simple shapes or as full textures. Scale acts similarly to the scaling mask options on VerdantObject, but is much more performant for dynamic objects. 

Each type needs a special component attached to the camera for tracking the affectors and storing how the world has been influenced. These are called fields.

## Fields

Fields work similarly to VerdantCamera in that they keep track of an abstracted version of the scene around it. Just like VerdantCamera they have a limited range which moves with the camera as it traverses the scene. Affectors can "mark" the field if they are in range, and the field will remember the marks for as long as they stay in range. As the camera moves marks that are out of range will be discarded, leaving space for new ones. 

The deflection field also runs a physics simulation to make pressed vegetation bounce back naturally. To support this it has a bunch of special parameters which you can read more about in its [component reference]().

## Affectors

Each affector component is slightly different, but they all share some commonalities. When you add an affector it will automatically add a VerdantShapeDescriptor, just like VerdantObject does. Affectors need to have a shape as it determines how their marks will look. You can use any mesh you like, though you might find that simple shapes like spheres or boxes often leave the clearest marks. Just like with colliders it's usually best to use the simplest shape you can! 

Affectors only leave marks when they are close to the ground, so a rolling ball will only influence its contact area and a falling pillar will leave a mark in its shape. The allowed distance to the ground can be set with the parameter Ground Distance, and for cases where you just want the affector to mark regardless you can set Ignore Height to true.

Deflection is, again, a little bit special. It has two different modes, Press and Push. A push is a transient force that can be strong or gentle, but it's only active while the affector is present. Press on the other hand can leave lasting marks and has a time parameter that controls how long vegetation stays down. Unlike push it can only ever do full presses, so it's best for things like heavy boxes or wheels that will flatten vegetation completely. Push should be used for gentler forces like wind or living creatures.

## Usage

Let's go over the process of adding a deflection affector to a game character. We'll start by adding a VerdantDeflectionField to the camera with VerdantCamera on it. As we're only really interested in the deflection around the main character we can keep the range as it is, so we won't touch the parameters for now.

Next, we need to add a new GameObject that follows the feet of the character. In this example I've simply made a child of the root object and moved it down one unit. On this object we add a VerdantDeflectionAffector. The shape doesn't need to be complicated, so on VerdantShapeDescriptor we set the mode to Primitive and the primitive to Sphere. If gizmos are enabled you should now see a sphere wireframe around the feet of the character.

We'll set the force mode to push and the force to 1. That will make it very clear if the affector is working or not, and we can tweak it to a lower strength later. The Direction can be set however you like, Move Direction often looks more natural for gentle forces whereas By Shape (which uses the mesh normals) can look a bit more like someone sweeping the vegetation aside around them.

You can test directly in the editor if you have Render in Editor enabled or you can just run the game. Either way you'll want to test in a field with pretty high (roughly knee-height) and dense grass, that's when deflection looks the most dramatic. You should see grass sweep away as your character walks through it!

If you don't, there are a few things you can try. Make sure the affector is close to or in the ground. You can increase the Ground Distance, or if nothing is working try setting Ignore Height and see if that helps. Also try increasing the Force or use the Press mode instead. You should also check your VerdantType. If Deflection Angle is low you can try increasing it. Double check that you added the VerdantDefletionField. If nothing works, try restarting Unity.

## Learning More

We've only covered the basics here and barely touched on Color and Scale at all, but you'll find that the basics are very similar to Deflection. You should experiment a bit with them, affectors are very fun to use and can add a lot of flair to your game. If you do want to dive in depth the Component Reference goes into detail on all the parameters, both for [affectors]() and [fields]().

Next, we'll take a look at some more advanced features of everything covered so far and use them to really make the visuals pop! There are a lot of subtle touches we can add that go a long way, often much more so than just increasing the density and adding more types. See you in [Visual Flair](VisualFlair.html).





Verdant supports three different affector fields: Deflection, Color and Scale. Deflection allows you to push vegetation aside or press it down below objects, whereas the other two do exactly whay you’d expect them to. They can be used for effects like cutting grass, rejuvenating withering flowers or to char the edges of an explosion.

All affector fields have two main parameters: The scale and the resolution. Together, these determine how far from the camera affectors will be able to touch the field, and at what level of detail the effect will be rendered. When the camera is moving it also determines for how long the field will remember changes made to it.

The deflection field is a simulation field and thus has a number of unique parameters. Read more about them on the Deflection Page.

Color and Scale are Textured Fields, which means they share a set of additional parameters.

A textured field can contain a base texture, which the field is initalized with and to which it will return if changed. Even if you don’t use affectors for interactivity a base texture can be very useful for adding variety to your scene. The images below show the difference between a plain field without base textures and one with some scale and color noise applied.

The scale of the base texture can be configured using the 

A base texture can also be remapped to new scales or colors. This is very useful when working with noise textures. For example, black and white texture can be remapped to the scale range 0.7-1.0, which will give a much subtler effect. 

When using a base texture you generally want the field texture mode to be set to one of the repeating options. Otherwise there will be a sharp divide where the field ends.

Unlike the deflection field, scale and color fields won’t return to their default state automatically when touched by an affector. You can enable them to do so by enabling restoration over time. 