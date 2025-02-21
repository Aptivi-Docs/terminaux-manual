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
