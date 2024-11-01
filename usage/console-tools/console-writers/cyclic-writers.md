---
description: Such writers render repeatedly with or without some movement
icon: arrow-rotate-right
---

# Cyclic Writers

Cyclic writers are dynamic writers that can be rendered individually by making a new class instance of a renderable class that implements the `IStaticRenderable` interface that you can implement in your renderable class. Such writers can either be animated or static, and can be rendered either by calling their individual `Render()` function one by one or by putting renderable classes to a container and calling that container's `WriteContainer()` function from the `ContainerTools` class. The following built-in cyclic writers are available:

* Shapes
  * `Circle`
  * `Ellipsis`
  * `Parallelogram`
  * `Rectangle`
  * `Square`
  * `Trapezoid`
  * `Triangle`
  * `Line`
* Charts
  * `BreakdownChart`
  * `BarChart`
  * `StickChart`
* Text
  * `AlignedFigletText`
  * `AlignedText`
  * `BoundedText`
  * `FigletText`
  * `TextMarquee`
* Artistic
  * `Border`
  * `Box`
  * `BoxFrame`
  * `Canvas`
* Misc
  * `Asciinema` (WIP)
  * `ProgressBar`
  * `ProgressBarNoText`
  * `Spinner`
    * Built-in spinners are available in the `BuiltinSpinners` class.
  * `Table`

You can define a container by creating a new instance of the `Container` class and adding some of the renderables that can be identified by their name. You can also set their positions by using the `SetRenderablePosition()` function.

{% hint style="info" %}
Some of the renderables may override the position variable for a renderable and may use the values from the renderables' properties.
{% endhint %}

In addition to that, you can manipulate with a renderable using the following functions:

* `IsRegistered()`: Checks to see if a renderable with this ID is registered or not.
* `RemoveRenderable()`: Removes a renderable with this ID.
* `GetRenderable()`: Gets a renderable instance from this ID.
* `GetRenderablePosition()`: Gets a renderable instance position from this ID.
* `GetRenderableNames()`: Gets an array of renderable IDs.

## Shapes

To be populated

### Circle

### Ellipsis

### Parallelogram

### Rectangle

### Square

### Trapezoid

### Triangle

## Charts

To be populated

### Breakdown chart

### Bar chart

### Stick chart

## Text

To be populated

### Aligned figlet text

### Aligned text

### Bounded text

### Figlet text

### Text marquee

## Artistic

To be populated

### Border

### Box

### Box frame

### Canvas

## Misc

To be populated

### Asciinema

### Progress bar

#### Progress bar with text

#### Progress bar without text

### Spinner

### Table
