---
layout: default
title: "VerdantGameObjectTwin"
parent: "Component Reference"
nav_order: "7"
---

# VerdantGameObjectTwin

To use a prefab as the [twin object](../AdvancedGuide/UsingTwinObjects) of a [VerdantType](DataTypes/VerdantType) it must have a component inheriting from this class. It specifies how the object should react to various events throughout its lifecycle. Because the class is abstract all of its methods must be overridden and fully implemented.

Verdant uses an object pooling system for twin objects to avoid unnecessary instantiation. GameObjects are instantiated once when first needed. Then, when they leave the twin object threshold they are reset, deactivated and returned to the pool. The next time an object needs to be placed Verdant checks the pool, and if it finds an object there it activates it and use it rather than instantiating a new one. 

For objects that consist of some combination of child transforms with mesh renderers, colliders and rigidbodies you can use the component VerdantPhysicalTwin on the root object. It will make sure all the rigidbodies and child transforms are reset correctly and that the objects are hidden when not in use. For more specific uses you will have to implement your own subclass.


## Abstract Methods

|:---------------|:--------------------------|
| `public abstract void Initiate()` | Called when the object is instantiated. All expensive operations that only need to run once should be performed here, like calling GetComponent or searching through children.  |
| `public abstract void Reset()` | Should reset the object to its initial state so it can be reused. If it doesn't restore it fully there's a chance that the object won't match the VerdantType and break the illusion that the GPU instance and its twin are one and the same. Reset is usually called right before the object is deactivated so it is in the right state when reactivated.   |
| `public abstract void Deactivate()` | Runs when the object is returned to the pool. Should deactivate it so that it cannot be seen or interacted with. In many cases you can simply disable the gameObject, but doing so can be quite costly for complex objects. It can often be better to disable its colliders and renderers, though the best solution will always depend on your particular usage. In general, anything that might influence the state set in Reset() must be disabled. |
| `public abstract void Activate()` | Runs when the object is retrieved from the pool to be placed back into the world. Whatever was done in Deactivate should be undone here so the object can be seen and interacted with again. |
| `public abstract void SetLayer(int layer)` | Runs when the object is retrieved from the pool to be placed back into the world or if the layer set on VerdantCamera changes at runtime. It should change the layer of the root object and any relevant children.  |
