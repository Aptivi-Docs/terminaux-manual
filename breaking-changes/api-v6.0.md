---
icon: up
description: Breaking changes for API v6.0
---

# API v6.0

Here is a list of breaking changes that happened during the API v6.0 period when differing versions of Terminaux introduced breaking changes.

## From 5.4.x to 6.0.x

Between the 5.4.x and 6.0.x version range, we've made the following breaking changes:

### `ConsoleFilterInfo` class added

{% code title="ConsoleFilter.cs" lineNumbers="true" %}
```csharp
public static bool IsInFilter(string query, ConsoleFilterType type, ConsoleFilterSeverity severity, out (Regex? query, ConsoleFilterType type, ConsoleFilterSeverity severity, string justification) queryTuple)
public static bool IsInFilter(Regex query, ConsoleFilterType type, ConsoleFilterSeverity severity, out (Regex? query, ConsoleFilterType type, ConsoleFilterSeverity severity, string justification) queryTuple)
public static (Regex? query, ConsoleFilterType type, ConsoleFilterSeverity severity, string justification)[] GetFilteredQueries()
```
{% endcode %}

The `ConsoleFilterInfo` class has been added to Terminaux's console filtering feature that lists all filter info, including the filter type, the query to match, and so on. This is to make the above functions easier to use by simplifying a tuple to a class.

{% hint style="info" %}
You'll have to change how your application uses these functions by changing the appropriate calls and references.
{% endhint %}

### `WideChar` moved to Textify

{% code title="WideChar.cs" lineNumbers="true" %}
```csharp
public struct WideChar : IEquatable<WideChar>, IComparable<WideChar>
```
{% endcode %}

We've moved `WideChar` struct to the latest version of Textify that Terminaux now uses. This is to enable more applications using this structure for wide characters.

{% hint style="info" %}
Just change the `using` clause to point to `Textify.General.Structures`.
{% endhint %}

### Centered writers refactored

{% code title="CenteredTextColor.cs" lineNumbers="true" %}
```csharp
public static class CenteredTextColor
{
    public static void WriteCentered(int top, string Text, int leftMargin = 0, int rightMargin = 0, params object[] Vars) { }
    public static void WriteCenteredColor(int top, string Text, Color Color, int leftMargin = 0, int rightMargin = 0, params object[] Vars) { }
    (...)
}
```
{% endcode %}

{% code title="CenteredFigletTextColor.cs" lineNumbers="true" %}
```csharp
public static class CenteredFigletTextColor
{
    public static void WriteCenteredFiglet(int top, FigletFont FigletFont, string Text, int leftMargin = 0, int rightMargin = 0, params object[] Vars) { }
    public static void WriteCenteredFigletColor(int top, FigletFont FigletFont, string Text, Color Color, int leftMargin = 0, int rightMargin = 0, params object[] Vars) { }
    (...)
}
```
{% endcode %}

The above centered writers have been replaced by more flexible aligned writers that allow you to not only write text to the console at the center of the console, but you can also align it either to the left, to the middle, or to the right.

* `CenteredTextColor` -> `AlignedTextColor`
* `CenteredFigletTextColor` -> `AlignedFigletTextColor`

{% hint style="info" %}
If you still want to write text at the center of the console, you can use the `TextAlignment.Middle` enumeration.
{% endhint %}

### Shapes and `IGeometricShape` moved to `CyclicWriters`

{% code title="IGeometricShape.cs" lineNumbers="true" %}
```csharp
namespace Terminaux.Graphics
```
{% endcode %}

{% code title="Shape source codes" lineNumbers="true" %}
```csharp
namespace Terminaux.Graphics.Shapes
```
{% endcode %}

As part of the renderable cyclic writers that can be contained in a single `Container` instance, we've moved all the shapes and their interface, `IGeometricShape`, as both that interface and `IStaticRenderable` work similarly to each other. The shape feature and its interface was a prelude to the static renderable writers.

{% hint style="info" %}
You just need to change the `using` clause to point to `Terminaux.Writer.CyclicWriters.Shapes`. You can still tell the difference between a static renderable and a geometric shape by examining the implemented interfaces, since we don't plan to remove the geometric shape interface.
{% endhint %}
