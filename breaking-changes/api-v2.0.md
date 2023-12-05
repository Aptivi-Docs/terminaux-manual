---
description: Breaking changes for API v2.0
---

# ⬆ API v2.0

Here is a list of breaking changes that happened during the API v2.0 period when differing versions of Terminaux introduced breaking changes.

## From 1.12.x to 2.0.x

Between the 1.12.x and 2.0.x version range, we've made the following breaking changes:

### Removed `Terminaux.Figgle`

Figgle was not maintained by the original developer for a long time, so we've forked this library to create Figletize, and its documentation can be found here:

{% content-ref url="http://127.0.0.1:5000/o/fj052nYlsxW9IdL3bsZj/s/ImPNBShiE2aqA6pDyeiZ/" %}
[Figletize - Manual](http://127.0.0.1:5000/o/fj052nYlsxW9IdL3bsZj/s/ImPNBShiE2aqA6pDyeiZ/)
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

Since the legacy FIglet code has been removed from Terminaux and the figlet selector was considered as an input method to select a Figlet font, we've decided to move the figlet selector to `Inputs.Styles`. This is to allow better organization.

{% hint style="info" %}
None of the functions have changed during this movement. You need to update your imports to point to `Terminaux.Inputs.Styles`.
{% endhint %}
