---
description: Breaking changes for API v7.0
icon: up
---

# API v7.0

Here is a list of breaking changes that happened during the API v7.0 period when differing versions of Terminaux introduced breaking changes.

## From 6.0.x to 7.0.x

Between the 6.0.x and 7.0.x version range, we've made the following breaking changes:

### Removed all obsolete functions

{% code title="Affected classes" lineNumbers="true" %}
```csharp
public static class GraphicsTools
public static class ListWriterColor
public static class AlignedFigletTextColor
public static class AlignedTextColor
public static class BarChartColor
public static class BorderColor
public static class BorderTextColor
public static class BoxColor
public static class BoxFrameColor
public static class BreakdownChartColor
public static class CanvasColor
public static class FigletColor
public static class FigletWhereColor
public static class PowerLineColor
public static class ProgressBarColor
public static class ProgressBarVerticalColor
public static class RainbowBackTextWriterColor
public static class RainbowTextWriterColor
public static class SliderColor
public static class SliderVerticalColor
public static class StickChartColor
public static class TableColor
public static class TruncatedLineText
public static class TruncatedText
public static class KeybindingsWriter
public static class LineHandleRangedWriter
public static class LineHandleWriter
```
{% endcode %}

The above classes, which were marked as deprecated back in Terminaux 6.0, have been removed in this API revision to clean things up as we're getting ready to stabilize the cyclic writers to make them more consistent and usable than before.

{% hint style="info" %}
You'll have to use cyclic writers if you want to continue using fancy writers. You can consult [this page](../usage/console-tools/console-writers/cyclic-writers.md) for more info.
{% endhint %}

### Base shell info generic class implemented

{% code title="BaseShellInfo.cs" lineNumbers="true" %}
```csharp
public abstract class BaseShellInfo : IShellInfo
```
{% endcode %}

When you create your own shell info classes to give your shell some important information, such as commands and other info, you had to override the `BaseShell` property so that it returned a new instance of your shell. However, this can be a hassle in certain scenarios.

Starting from Nitrocid KS 0.1.2 and Terminaux 7.0, you'll no longer have to override the BaseShell property because we've implemented a generic version of the above class that takes a type of your shell. For example, if your shell is called `OxygenShell` and your shell info class is called `OxygenShellInfo`, you can just inherit the generic version of the class so that it becomes `BaseShellInfo<OxygenShell>`.

As a result, the following properties are removed from the IShellInfo interface:

{% code title="IShellInfo.cs" lineNumbers="true" %}
```csharp
BaseShell? ShellBase { get; }
PromptPresetBase CurrentPreset { get; }
```
{% endcode %}

{% hint style="info" %}
When inheriting the non-generic base shell class, your shell info class might hold wrong information about your shell, even if your commands are defined. Therefore, you must migrate to the generic version of the class if you want to retain your shell settings.
{% endhint %}

### Cyclic writers now use typed `CyclicWriter` classes

The cyclic writers have been redefined to make their classes use the typed `CyclicWriter` classes. These classes consist of:

* `SimpleCyclicWriter`
* `GraphicalCyclicWriter`

This was required because we needed applications to be able to determine whether the writer that they're using was a simple one or a graphical one. Simple cyclic writers are suitable for command line applications, while graphical ones are used in TUIs.

The following writers have been migrated to `SimpleCyclicWriter`:

* `Decoration`
* `Emoji`
* `FigletText`
* `Kaomoji`
* `KeyShortcut`
* `Keybindings`
* `LineHandle`
* `ListEntry`
* `Listing`
* `NerdFonts`
* `PowerLine`
* `ProgressBar`
* `ProgressBarNoText`
* `SimpleProgress`
* `Slider`
* `Spinner`
* `StemLeafChart`
* `TextException`
* `TextMarquee`

The following writers have been migrated to `GraphicalCyclicWriter`:

* `AlignedFigletText`
* `AlignedText`
* `AnimatedCanvas`
* `AnimatedText`
* `BarChart`
* `Border`
* `BoundedText`
* `Box`
* `BoxFrame`
* `BreakdownChart`
* `Calendars`
* `Canvas`
* `Eraser`
* `Line`
* `LineChart`
* `LinesChart`
* `PieChart`
* `Selection`
* `StickChart`
*  `SyntaxText`
* `Table`
* `TextPath`
* `WinsLosses`

The following shape renderers have been migrated to `GraphicalCyclicWriter`:

* `Arc`
* `Circle`
*  `Ellipsis`
* `Parallelogram`
* `Rectangle`
* `Square`
* `Trapezoid`
* `Triangle`

Part of the introduction of the two typed cyclic writer classes involved removing both `IStaticRenderable` and `IGeometricShape` interfaces to reduce the complexity involved.

{% hint style="info" %}
Consult the updated cyclic writer documentation for more information.
{% endhint %}

### Changed left and right margins to widths in some graphical writers

{% code title="Some graphical writers" lineNumbers="true" %}
```csharp
public int LeftMargin
{
    get => leftMargin;
    set => leftMargin = value;
}

public int RightMargin
{
    get => rightMargin;
    set => rightMargin = value;
}
```
{% endcode %}

We have replaced the two above properties with the `Width` property that some graphical writers that are derived from `GraphicalCyclicWriter` use to determine the element width. The following writers are affected:

* `FigletText`
* `ProgressBar`
* `ProgressBarNoText`
* `SimpleProgress`
* `TextMarquee`

{% hint style="info" %}
You'll have to calculate the total width that the affected writers need to render to the terminal.
{% endhint %}

### `Keybindings` and `StemLeafChart` writers now act as a simple writer

{% code title="Keybindings.cs + StemLeafChart.cs" lineNumbers="true" %}
```csharp
public int Left { get; set; }
public int Top { get; set; }
```
{% endcode %}

The two above properties have been removed because simple writers are not supposed to use anything related to positioning.

{% hint style="info" %}
You can still use the rendering tools functions to accommodate your needs, should you use positioning to write these.
{% endhint %}

### Moved `WriteRenderable()` and `RenderRenderable()` to `RendererTools`

We have moved all `WriteRenderable()` and `RenderRenderable()` function overloads from `ContainerTools` to `RendererTools` to better convey the goals as we have created the two base cyclic writer classes.
