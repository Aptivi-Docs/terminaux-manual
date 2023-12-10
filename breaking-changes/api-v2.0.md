---
description: Breaking changes for API v2.0
---

# ⬆ API v2.0

Here is a list of breaking changes that happened during the API v2.0 period when differing versions of Terminaux introduced breaking changes.

## From 1.12.x to 2.0.x

Between the 1.12.x and 2.0.x version range, we've made the following breaking changes:

### Removed `Terminaux.Figgle`

Figgle was not maintained by the original developer for a long time, so we've forked this library to create Figletize, and its documentation can be found here:

{% content-ref url="https://app.gitbook.com/o/fj052nYlsxW9IdL3bsZj/s/ImPNBShiE2aqA6pDyeiZ/" %}
[Figletize - Manual](https://app.gitbook.com/o/fj052nYlsxW9IdL3bsZj/s/ImPNBShiE2aqA6pDyeiZ/)
{% endcontent-ref %}

{% hint style="info" %}
It's advisable to use the built-in Figlet tools instead of using Terminaux.Figgle as it's considered legacy.
{% endhint %}

### Removed the old color wheel

The old color wheel wasn't updated to benefit from the latest Terminaux improvements, so we had to remove the old color wheel to reduce the maintenance burden.

{% hint style="info" %}
It's advisable to use the newer color selector as it's more up to date.
{% endhint %}

### Implemented a proper console wrapper

{% code title="ConsoleWrappers.cs" lineNumbers="true" %}
```csharp
public static class ConsoleWrappers
```
{% endcode %}

A proper console wrapper was needed to effortlessly call the console wrapper functions that Terminaux uses to delegate the `Console` functions so that appropriate writing methods are used. This is done in an attempt to achieve consistency in behavior across all the Terminaux functions.

Although the action-based console wrappers can still be set, you can no longer call them from your application. Instead, you must use the newer `ConsoleWrapper` class to more accurately represent the properties and the functions and to avoid confusion.

{% hint style="info" %}
To be able to achieve a successful migration, you must replace the following calls (left is `ConsoleWrapperTools`, and right is `ConsoleWrapper`):

* `ActionCursorLeft` -> `CursorLeft`
* `ActionSetCursorLeft` -> `CursorLeft`
* `ActionCursorTop` -> `CursorTop`
* `ActionSetCursorTop` -> `CursorTop`
* `ActionBufferHeight` -> `BufferHeight`
* `ActionCursorVisible` -> `CursorVisible`
* `ActionKeyAvailable` -> `KeyAvailable`
* `ActionClear` -> `Clear`
* `ActionSetCursorPosition` -> `SetCursorPosition`
* `ActionBeep` -> `Beep`
* `ActionReadKey` -> `ReadKey`
* `ActionWriteChar` -> `Write`
* `ActionWriteString` -> `Write`
* `ActionWriteParameterized` -> `Write`
* `ActionWriteLine` -> `WriteLine`
* `ActionWriteString` -> `WriteLine`
* `ActionWriteLineParameterized` -> `WriteLine`
{% endhint %}

### Textify is used for VT sequence feature

{% code title="Affected classes" lineNumbers="true" %}
```csharp
public static class ApcSequences
public static class C1Sequences
public static class CsiSequences
public static class DcsSequences
public static class EscSequences
public static class OscSequences
public static class PmSequences
public static class VtSequenceBasicChars
public static class VtSequenceBuilderTools
public enum VtSequenceSpecificTypes
public static class VtSequenceRegexes
public static class VtSequenceTools
public enum VtSequenceType
```
{% endcode %}

Since Textify was released as a brand new library for .NET, which was a library that moved all text-related tools from various libraries of our own, we've decided to move these classes to Textify, removing them from Terminaux entirely. This is to allow diversity of development for the two libraries and to allow interoperability.

{% hint style="info" %}
Terminaux will pull Textify as a dependency when you install the 2.0 version, so you just have to change the usings to Textify's namespace before you can use the above classes again.

If you're impatient for the next version of Terminaux, you can install Textify manually and use these functions. Be aware that you may need to resolve the conflicts.
{% endhint %}

### Moved `Inputs` to the root namespace

{% code title="Affected files" lineNumbers="true" %}
```csharp
namespace Terminaux.Reader.Inputs
```
{% endcode %}

`Inputs` hosted several of the input tools to allow you to ask users a question in several of the forms, including the selection choices, the modal infoboxes, and so on.

However, we've moved the entire `Inputs` namespace to `Terminaux.Inputs` as it just uses the console reader that Terminaux implements and not modifies it.

{% hint style="info" %}
None of the classes and their functions have changed. You just have to update the imports to use `Terminaux.Inputs` instead of the old namespace shown above.
{% endhint %}

### Figlet selector moved to `Inputs.Styles`

{% code title="FigletSelector.cs" lineNumbers="true" %}
```csharp
namespace Terminaux.Figlet
```
{% endcode %}

Since the legacy Figlet code has been removed from Terminaux and the figlet selector was considered as an input method to select a Figlet font, we've decided to move the figlet selector to `Inputs.Styles`. This is to allow better organization.

{% hint style="info" %}
None of the functions have changed during this movement. You need to update your imports to point to `Terminaux.Inputs.Styles`.
{% endhint %}

### Removed `ColorSeqException`

{% code title="ColorSeqException.cs" lineNumbers="true" %}
```csharp
internal class ColorSeqException
```
{% endcode %}

ColorSeq was replaced by Terminaux, because the latter library works on managing your terminal applications by providing you several of the nice terminal tools, such as the efficient management of colors, input reading, and much more.

We've removed `ColorSeqException` as an internal class, but we need to put this change here to tell the developers that they can finally handle the `Color` errors easily by catching all `TerminauxException` errors.

{% hint style="info" %}
You no longer need to use reflection to find out the type name of the removed exception, because you can catch `TerminauxException`, which the color management system now uses.
{% endhint %}

### `ConsolePlatform`'s `NewLine`

{% code title="ConsolePlatform.cs" lineNumbers="true" %}
```csharp
public static string NewLine
```
{% endcode %}

From now on, this property has been removed as it isn't used except the console checker. Also, the usage of Textify in the v2.0 version of Terminaux means that you need not to resort to cryptic hacks to get the new lines working properly.

{% hint style="info" %}
If you still make use of this property, replace its call with either the `\n` string literal (which represents a new line and is platform-agnostic), or use `Environment.NewLine` if you want to use platform-specific newlines.
{% endhint %}

### Relocated color model conversion tools

{% code title="Color.cs" lineNumbers="true" %}
```csharp
public CyanMagentaYellowKey CMYK =>
    RGB.ConvertToCmyk();
(...)
```
{% endcode %}

The color model conversion tools have been reworked to become easier to use than the Terminaux v1.x version series, which use constructors that do the conversion. It was discovered that it was not so easy to maintain, so we've decided to relocate these to their own dedicated classes, such as CMY conversion tools (`CmyConversionTools`), so that they can be used by Terminaux v2.0 applications.

They are also titled appropriately so that you can better understand what is the source and what is the target unit being used to convert the source color model to. More documentation is found in its appropriate page.

{% hint style="info" %}
Change all the calls to the color model-specific properties to refer to the conversion tools that focus on a target that you want. For example, converting RGB to CMY requires usage of the `CmyConversionTools` class.
{% endhint %}
