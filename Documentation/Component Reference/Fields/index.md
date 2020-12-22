---
layout: default
title: "Fields"
parent: "Component Reference"
nav_order: "5"
has_children: true
---

# Fields

Fields are simplified representations of the game world that Verdant uses to place and animate vegetation. The three in this category are designed to facilitate interaction between dynamic objects and Verdant vegetation. 

All field componenets should be attached to the same GameObject as the VerdantCamera, and if there are multiple VerdantCameras in the scene all of them will need field components to be able to see field interactions. 

Fields are connected to Affectors, which paint information into them. If there is no field component on the VerdantCamera there is nothing for the affectors to paint into, so they won't influence your vegetation. Similarly, if there is a field but no affectors the field will be in its default state and contribute nothing.

There is a certain performance cost associated with all fields. It is highly dependent on the range, resolution and number of affectors used, but even just adding a field component will make the shader take a slightly more expensive path. Make sure you only use fields that you actually need, and tweak their parameters carefully to the minimum requirements of your game. If you are having issues, try disabling your field components to see what effect they have on frame time.

Like most components in Verdant, field parameters are accessible from script as well as the inspector. As a rule they should not be changed at runtime or you will risk big framerate hitches as the field rebuilds, but they can be very useful for implementing things like graphics settings that only change occasionally.