---
layout: default
title: "Affectors"
parent: "Component Reference"
nav_order: "4"
has_children: true
---

# Affectors

Affectors in Verdant are components that inflence their corresponding fields. This can mean adding color to vegetation, changing its scale or making it bend around a rolling ball. Each affector can be configured separately and they will render to the field as they move around. Affectors are designed to be used with dynamic objects, so you can add them to a moving character, the arc of a sword or anything else you might want to use to interact with Verdant. While you should always be mindful of not using too many they are culled outside the range of the field, so keeping them performant is more about density than absolute numbers. 

By default, affectors only paint to fields when they are close to the ground. This can be adjusted with the Ground Distance parameter or disabled entirely with Ignore Height.

All affectors have a [VerdantShapeDescriptor](../VerdantShapeDescriptor.html) component attached to them which determines their shape. This can be either the mesh of the object, a different mesh or one of a number of provided primitives. More than one affector can share a VerdantShapeDescriptor, so it's possible to for example both deflect and set a color in the same shape without duplicating it. For more details, see the [VerdantShapeDescriptor](../VerdantShapeDescriptor.html) documentation.