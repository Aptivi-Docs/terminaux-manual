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

### Merged four console blacklists/graylists

{% code title="TerminalEmulatorBlacklist.cs" lineNumbers="true" %}
```csharp
public static class TerminalEmulatorBlacklist
```
{% endcode %}

{% code title="TerminalEmulatorGreylist.cs" lineNumbers="true" %}
```csharp
public static class TerminalEmulatorGreylist
```
{% endcode %}

{% code title="TerminalTypeBlacklist.cs" lineNumbers="true" %}
```csharp
public static class TerminalTypeBlacklist
```
{% endcode %}

{% code title="TerminalTypeGreylist.cs" lineNumbers="true" %}
```csharp
public static class TerminalTypeGreylist
```
{% endcode %}

We have previously made the blacklist and the graylist for both the terminal types and the terminal emulators. However, each time we need to solve a bug or make an improvement in one of them, we'll have to update all of them to follow suit to avoid inconsistencies in behavior.

As a result, we've merged them to a single class, called `ConsoleFilter`, that allows you to manage your console filters without any hitch. We've introduced two enums that will be listed in the console checker page.

{% hint style="info" %}
You'll have to update your references to point to the above class and to make use of both the enums.
{% endhint %}

### Merged TermInfo to the main library

{% code title="All TermInfo sources" lineNumbers="true" %}
```csharp
namespace Terminaux.TermInfo
```
{% endcode %}

All TermInfo source code has been moved to the main library to make it more powerful. The reasons will be listed under a non-ordered list. This changes all  TermInfo classes to the `Terminaux.Base.TermInfo` namespace.

* The main Terminaux library originally included the TermInfo library as a separate NuGet package before being moved to the Terminaux source code. This caused conflicts as we're trying to move all the TermInfo sources to the main library under `Terminaux.Base`. We've removed this dependency to maintain consistency and to reduce conflicts.
* The idea that using an original TermInfo.Cli application in a script intended to download an NCurses library source code to extract a single file, called `Caps`, and to generate source code files under the same application for use with Terminaux's TermInfo is absurd, as users will have to turn on PowerShell script execution manually. Luckily, you can turn on the PowerShell script execution for the process's lifetime. There are Roslyn source generators for this very reason.
* We've maintained the `Caps` file unmodified from the [NCurses 6.4 source code](https://ftp.gnu.org/pub/gnu/ncurses/ncurses-6.4.tar.gz) for use with our source generator that generates these files without downloading anything off the Internet except NuGet packages. This ensures faster generation (in case NCurses developers decided to modify the Caps file), especially if your build server is disconnected from the Internet. We're trying to cater to the most general and the most simple build methods possible.
* We've moved the Inspect feature from the original TermInfo.Cli application to our demo application to be able to use it in the form of an interactive TUI. We've also imported all workable terminfo files from the minimal installation of Ubuntu as embedded resources.
* This required us to change the whole Terminaux library to be more nullable-friendly, but we don't currently have plans to make all our libraries nullable-friendly.

However, with respect to the original authors from Spectre.Console's [TermInfo](https://github.com/spectreconsole/terminfo), we've decided to preserve the original license file from that project.

{% hint style="info" %}
None of the functions have been changed. You'll just have to update the namespace imports to point to `Terminaux.Base.TermInfo`.

As for NCurses, you can consult [this page](https://ftp.gnu.org/pub/gnu/ncurses/) to check to see if there are any newer releases. NCurses doesn't get updated too frequently, and _may_ or _may not_ be updated either annually or bi-annually.
{% endhint %}

### Progress infobox's `waitForInput` removed

{% code title="InfoBoxProgressColor.cs" lineNumbers="true" %}
```csharp
public static void WriteInfoBoxProgress(double progress, string text, bool waitForInput, params object[] vars)
(...)
```
{% endcode %}

waitForInput was accidentally added to almost all the progress information box functions. This variable was possibly a leftover from the regular infobox that provides this variable, which tells the informational box to wait for the user input to make it a modal dialog box.

{% hint style="info" %}
You may have to remove boolean values for this argument. Otherwise, none of the functions have been functionally affected by this change.
{% endhint %}

### Interactive TUI functions now made generic

{% code title="IInteractiveTUI.cs" lineNumbers="true" %}
```csharp
public string GetEntryFromItem(object item);
public string GetInfoFromItem(object item);
public void RenderStatus(object item);
```
{% endcode %}

{% code title="BaseInteractiveTui.cs" lineNumbers="true" %}
```csharp
public virtual string GetEntryFromItem(object item)
public virtual string GetInfoFromItem(object item)
public virtual void RenderStatus(object item)
```
{% endcode %}

As a followup to the interactive TUI interface and its associated base class, we've made further modifications to the interactive TUI class by making the three above functions use the generic type instead of relying on the `object` type. This eliminates all the necessary casting before being able to use the value directly.

{% hint style="info" %}
You'll have to update your overrides to point to your interactive TUI's data type specified in the beginning of its own class file.
{% endhint %}

### Console color enumeration auto-generated

{% code title="ConsoleColors.cs" lineNumbers="true" %}
```csharp
public enum ConsoleColors
```
{% endcode %}

To follow-up the migration of the console color data class generation with the internal generator that parses the console color data JSON file from the resources and makes a data class based on it, we've decided to include the `ConsoleColors` enumeration to the generation process. This causes some of the names to be changed.

{% hint style="info" %}
The following 4-bit color names have been changed:

* `ConsoleColors.DarkYellow` -> `Olive`
* `ConsoleColors.Gray` -> `Silver`
* `ConsoleColors.DarkGray` -> `Grey`
* `ConsoleColors.Magenta` -> `Fuchsia`
* `ConsoleColors.Cyan` -> `Aqua`

The rest of the colors that have their names changed now have their own `-Alt` suffix instead of the color hex as the suffix.
{% endhint %}

### Renamed the color types

{% code title="ColorType.cs" lineNumbers="true" %}
```csharp
public enum ColorType
{
    (...)
    _255Color,
    _16Color,
}
```
{% endcode %}

To align with our goals for Terminaux 3.0 and to make user experience less confusing, we've decided to rename the two color types to their new names listed below in the hint box.

{% hint style="info" %}
The following color types have been renamed:

* `ColorType._255Color` -> `EightBitColor`
* `ColorType._16Color` -> `FourBitColor`
{% endhint %}
