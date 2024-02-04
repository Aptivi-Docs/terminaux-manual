---
description: Breaking changes for API v3.0
---

# ⬆ API v3.0

Here is a list of breaking changes that happened during the API v3.0 period when differing versions of Terminaux introduced breaking changes.

## From 2.7.x to 3.0.x

Between the 2.7.x and 3.0.x version range, we've made the following breaking changes:

### Console extension separation

{% code title="ConsoleExtensions.cs" lineNumbers="true" %}
```csharp
public static class ConsoleExtensions
```
{% endcode %}

The console extensions class needed to be separated to several classes to categorize the functions according to their type. This allows easier organization and distinguishing from the common Terminaux console functions from their extensions.

As a result, we've introduced several extensions that you can see here:

{% content-ref url="../usage/console-tools/console-extensions.md" %}
[console-extensions.md](../usage/console-tools/console-extensions.md)
{% endcontent-ref %}

{% hint style="info" %}
Consult the above page for functions and their categories to migrate from `ConsoleExtensions`.
{% endhint %}

### Brightness and contrast moved to `Transformation.Contrast`

{% code title="Affected classes" lineNumbers="true" %}
```csharp
namespace Terminaux.Colors
```
{% endcode %}

We've moved all the brightness and the contrast classes and enumerations to the `Transformation` namespace. This is to ensure better organzation. The following classes are affected:

* `ColorBrightness`
* `ColorContrast`
* `ColorContrastType`

{% hint style="info" %}
You'll have to update your using clauses to `Terminaux.Colors.Transformation.Contrast` when calling any of these classes.
{% endhint %}

### Moved plain writing functions to `TextWriterRaw`

{% code title="TextWriterColor.cs" lineNumbers="true" %}
```csharp
public static void Write()
public static void WritePlain(string Text, params object[] vars)
public static void WritePlain(string Text, bool Line, params object[] vars)
public static void WritePlain(string Text, TermReaderSettings settings, params object[] vars)
public static void WritePlain(string Text, TermReaderSettings settings, bool Line, params object[] vars)
```
{% endcode %}

We've made a breaking change that moves all the raw writing to its own class to reduce confusion. The standard console writer, `TextWriterColor`, sets the colors regardless of the mode, while the plain writers are more focused on writing console sequences and other raw things to the terminal.

{% hint style="info" %}
Their names have not been changed, but they have been moved to `TextWriterRaw`, so you'll need to update the references to these functions to point to that class instead of the `TextWriterColor` class.
{% endhint %}
