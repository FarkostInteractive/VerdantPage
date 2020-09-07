Affectors

To interact with vegetation you want to use an affector.

Verdant supports three different affector fields: Deflection, Color and Scale. Deflection allows you to push vegetation aside or press it down below objects, whereas the other two do exactly whay you’d expect them to. They can be used for effects like cutting grass, rejuvenating withering flowers or to char the edges of an explosion.

All affector fields have two main parameters: The scale and the resolution. Together, these determine how far from the camera affectors will be able to touch the field, and at what level of detail the effect will be rendered. When the camera is moving it also determines for how long the field will remember changes made to it.

The deflection field is a simulation field and thus has a number of unique parameters. Read more about them on the Deflection Page.

Color and Scale are Textured Fields, which means they share a set of additional parameters.

A textured field can contain a base texture, which the field is initalized with and to which it will return if changed. Even if you don’t use affectors for interactivity a base texture can be very useful for adding variety to your scene. The images below show the difference between a plain field without base textures and one with some scale and color noise applied.

The scale of the base texture can be configured using the 

A base texture can also be remapped to new scales or colors. This is very useful when working with noise textures. For example, black and white texture can be remapped to the scale range 0.7-1.0, which will give a much subtler effect. 

When using a base texture you generally want the field texture mode to be set to one of the repeating options. Otherwise there will be a sharp divide where the field ends.

Unlike the deflection field, scale and color fields won’t return to their default state automatically when touched by an affector. You can enable them to do so by enabling restoration over time. 