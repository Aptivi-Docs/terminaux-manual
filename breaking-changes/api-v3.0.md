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

### Migrated input functions to `TermReader`

{% code title="Input.cs" lineNumbers="true" %}
```csharp
public static TermReaderSettings GlobalReaderSettings
public static string CurrentMask
public static string ReadLine(bool interruptible = true)
public static string ReadLine(string InputText, bool interruptible = true)
public static string ReadLine(string InputText, string DefaultValue, bool interruptible = true)
public static string ReadLine(string InputText, string DefaultValue, TermReaderSettings settings, bool interruptible = true)
public static string ReadLineWrapped(bool interruptible = true)
public static string ReadLineWrapped(string InputText, bool interruptible = true)
public static string ReadLineWrapped(string InputText, string DefaultValue, bool interruptible = true)
public static string ReadLineWrapped(string InputText, string DefaultValue, TermReaderSettings settings, bool interruptible = true)
public static string ReadLine(string InputText, string DefaultValue, bool OneLineWrap = false, bool interruptible = true)
public static string ReadLine(string InputText, string DefaultValue, bool OneLineWrap = false, TermReaderSettings settings = null, bool interruptible = true)
public static string ReadLineNoInput(bool interruptible = true)
public static string ReadLineNoInput(TermReaderSettings settings, bool interruptible = true)
public static string ReadLineNoInput(char MaskChar, bool interruptible = true)
public static string ReadLineNoInput(char MaskChar, TermReaderSettings settings, bool interruptible = true)
```
{% endcode %}

We've discovered that all the `ReadLine()` functions and their associated properties and function overloads were duplicates of the TermReader's `Read()` function, so we've decided to migrate them to the `TermReader` class to avoid inconsistencies for both the application developer and the future versions of Terminaux.

{% hint style="info" %}
You'll have to refer to the `Read()` functions found in the `TermReader` class. You may have to change how to call the function by changing the signatures.
{% endhint %}

### Removed `ConsolePlatform`

{% code title="ConsolePlatform.cs" lineNumbers="true" %}
```csharp
public static class ConsolePlatform
```
{% endcode %}

All of its functions have been moved to SpecProbe's Software, so this entire class has been removed.

{% hint style="info" %}
As Terminaux already makes use of SpecProbe's Software, you should only change the reference to `ConsolePlatform` to point to `PlatformHelper`.
{% endhint %}

### Moved RGB value conversion tools

{% code title="ColorTools.cs" lineNumbers="true" %}
```csharp
public static double SRGBToLinearRGB(int colorNum)
public static int LinearRGBTosRGB(double linear)
```
{% endcode %}

The sRGB and the linear RGB conversion tools have been moved to the transformation tools as they were used for color transformation. They used to be in the `ColorTools` class, but they were used for color transformation formulas, such as color blindness simulation.

{% hint style="info" %}
These functions are not affected. You just have to update your references to `ColorTools` for these functions to point to `TransformationTools`.
{% endhint %}

### Moved animated writers to `DynamicWriters`

{% code title="All affected classes" lineNumbers="true" %}
```csharp
namespace Terminaux.Writer.ConsoleWriters
```
{% endcode %}

The following animated writers have been moved to their own category, `DynamicWriters`:

* `TextWriterSlowColor`
* `TextWriterWhereSlowColor`
* `TextWriterWrappedColor`

{% hint style="info" %}
You must update the usings clause to `Terminaux.Writer.DynamicWriters` to continue using the three above functions.
{% endhint %}

### Removed the `Input` class

{% code title="Input.cs" lineNumbers="true" %}
```csharp
public static class Input
{
    public static (ConsoleKeyInfo result, bool provided) ReadKeyTimeout(bool Intercept, TimeSpan Timeout)
    public static ConsoleKeyInfo DetectKeypress()
}
```
{% endcode %}

As the legacy Input class was made empty by the moving the associated `Read*()` functions to `TermReader`, we've decided to remove the entire class and move all of the functions to that class.

During the migration, we've renamed `DetectKeypress()` to `ReadKey()` and added an overload to determine whether we're intercepting the pressed key. The default value is still `true`.

{% hint style="info" %}
If you want to continue using these functions, you'll have to follow the upgrade paths:

* `Input.ReadKeyTimeout()` -> `TermReader.ReadKeyTimeout()`
* `Input.DetectKeypress()` -> `TermReader.ReadKey()`
{% endhint %}

### Moved the color selector to `Inputs`

{% code title="ColorSelector.cs" lineNumbers="true" %}
```csharp
namespace Terminaux.Colors.Selector
```
{% endcode %}

The color selector class, `ColorSelector`, has been moved from `Colors.Selector` to `Inputs.Styles` as it has been considered to be one of the input styles. This makes it for easier organization of the input styles, such as selection and infoboxes.

{% hint style="info" %}
The class wasn't renamed. You'll have to update the usings clause to use the new namespace.
{% endhint %}

### Interactive TUIs are now generic

{% code title="BaseInteractiveTui.cs" lineNumbers="true" %}
```csharp
public class BaseInteractiveTui : IInteractiveTui
public virtual IEnumerable PrimaryDataSource => Array.Empty<string>();
public virtual IEnumerable SecondaryDataSource => Array.Empty<string>();
public static BaseInteractiveTui Instance
```
{% endcode %}

{% code title="IInteractiveTui.cs" lineNumbers="true" %}
```csharp
public interface IInteractiveTui
public IEnumerable PrimaryDataSource { get; }
public IEnumerable SecondaryDataSource { get; }
```
{% endcode %}

{% code title="InteractiveTuiTools.cs" lineNumbers="true" %}
```csharp
public static void OpenInteractiveTui(BaseInteractiveTui interactiveTui)
public static void SelectionMovement(BaseInteractiveTui interactiveTui, int pos)
```
{% endcode %}

Non-generic IEnumerables tend to be more difficult to work with than the generic ones. Therefore, we've decided to switch to using generic IEnumerables. This means that the interactive TUI interface and the base class have become generic.

As a consequence, you'll have to either provide an exact type that your interactive TUI will work with, such as `string` or `int`, or you'll have to use `object`.

{% hint style="info" %}
In order to migrate to the new definition, just place a type in both the `BaseInteractiveTui` and the `IInteractiveTui` clauses in the beginning of your TUI class. Additionally, place a type in your `IEnumerable` definition.
{% endhint %}

### Removed `InputStyle`

{% code title="InputStyle.cs" lineNumbers="true" %}
```csharp
public static class InputStyle
```
{% endcode %}

To maintain consistency, we've decided to remove the entire class. This is because this class was considered legacy.

{% hint style="info" %}
You should use printing functions that Terminaux implements with a call to `TermReader.Read()` instead.
{% endhint %}

### Screen now handles clearing the console

{% code title="ScreenTools.cs" lineNumbers="true" %}
```csharp
public static void Render(bool clearScreen = false)
public static void Render(Screen screen, bool clearScreen = false)
```
{% endcode %}

Starting from Terminaux 3.0, the `clearScreen` variable is no longer needed as the screen feature can now figure out when to or not to clear the screen without your screen instance handling it yourself.

However, both local and global configuration will allow you to control whether to enable or to disable automatic handling of clearing the console in some situations where the default clear handler is not feasible enough.

{% hint style="info" %}
Just remove the clearScreen argument when calling `Render()`.
{% endhint %}

### `InputChoiceTools` now returns an array of `InputChoiceInfo`

{% code title="InputChoiceTools.cs" lineNumbers="true" %}
```csharp
public static List<InputChoiceInfo> GetInputChoices(string AnswersStr, string[] AnswersTitles)
public static List<InputChoiceInfo> GetInputChoices(string[] Answers, string[] AnswersTitles)
```
{% endcode %}

All of the `InputChoiceTools`, as well as all the input style class functions that use a list of `InputChoiceInfo`, have their signatures changed to return an array of `InputChoiceInfo` as this return value is not meant to be modified.

{% hint style="info" %}
Change all the `InputChoiceInfo` lists to arrays before using them in the input functions, such as the selection style.
{% endhint %}
