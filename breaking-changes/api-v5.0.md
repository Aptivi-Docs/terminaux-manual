---
description: Breaking changes for API v5.0
---

# ⬆️ API v5.0

Here is a list of breaking changes that happened during the API v5.0 period when differing versions of Terminaux introduced breaking changes.

## From 4.3.x to 5.0.x

Between the 4.3.x and 5.0.x version range, we've made the following breaking changes:

### ChoiceStyle simplified

{% code title="ChoiceStyle.cs" lineNumbers="true" %}
```csharp
public static string PromptChoice(string Question, (string, string)[] Answers, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, (string, string)[] Answers, (string, string)[] AlternateAnswers, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, InputChoiceInfo[] Answers, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, InputChoiceInfo[] Answers, InputChoiceInfo[] AltAnswers, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, (string, string)[] Answers, Color questionColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, (string, string)[] Answers, (string, string)[] AlternateAnswers, Color questionColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, InputChoiceInfo[] Answers, Color questionColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, InputChoiceInfo[] Answers, InputChoiceInfo[] AltAnswers, Color questionColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, (string, string)[] Answers, Color questionColor, Color inputColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, (string, string)[] Answers, (string, string)[] AlternateAnswers, Color questionColor, Color inputColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, InputChoiceInfo[] Answers, Color questionColor, Color inputColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, InputChoiceInfo[] Answers, InputChoiceInfo[] AltAnswers, Color questionColor, Color inputColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, (string, string)[] Answers, Color questionColor, Color inputColor, Color optionColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, (string, string)[] Answers, (string, string)[] AlternateAnswers, Color questionColor, Color inputColor, Color optionColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, InputChoiceInfo[] Answers, Color questionColor, Color inputColor, Color optionColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, InputChoiceInfo[] Answers, InputChoiceInfo[] AltAnswers, Color questionColor, Color inputColor, Color optionColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, (string, string)[] Answers, Color questionColor, Color inputColor, Color optionColor, Color altOptionColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, (string, string)[] Answers, (string, string)[] AlternateAnswers, Color questionColor, Color inputColor, Color optionColor, Color altOptionColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, InputChoiceInfo[] Answers, Color questionColor, Color inputColor, Color optionColor, Color altOptionColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, InputChoiceInfo[] Answers, InputChoiceInfo[] AltAnswers, Color questionColor, Color inputColor, Color optionColor, Color altOptionColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, (string, string)[] Answers, Color questionColor, Color inputColor, Color optionColor, Color altOptionColor, Color disabledOptionColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, (string, string)[] Answers, (string, string)[] AlternateAnswers, Color questionColor, Color inputColor, Color optionColor, Color altOptionColor, Color disabledOptionColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, InputChoiceInfo[] Answers, Color questionColor, Color inputColor, Color optionColor, Color altOptionColor, Color disabledOptionColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
public static string PromptChoice(string Question, InputChoiceInfo[] Answers, InputChoiceInfo[] AltAnswers, Color questionColor, Color inputColor, Color optionColor, Color altOptionColor, Color disabledOptionColor, ChoiceOutputType OutputType = ChoiceOutputType.OneLine, bool PressEnter = false)
```
{% endcode %}

We've made a new class, `ChoiceStyleSettings`, to ease configuration of the choice style. This results in the above function overloads being removed so that all overloads take an argument of `ChoiceStyleSettings`. This argument defines the very settings that you used to define in these overloads to change how it's rendered, including the colors.

{% hint style="info" %}
You must move all the configuration, including the colors, to the `ChoiceStyleSettings` instance to be able to continue configration.
{% endhint %}

### Table renderer re-write

{% code title="TableColor.cs" lineNumbers="true" %}
```csharp
public static void WriteTablePlain(string[] Headers, string[,] Rows, int Margin, bool SeparateRows = true, List<CellOptions>? CellOptions = null)
public static void WriteTable(string[] Headers, string[,] Rows, int Margin, bool SeparateRows = true, List<CellOptions>? CellOptions = null)
public static void WriteTable(string[] Headers, string[,] Rows, int Margin, Color SeparatorForegroundColor, Color HeaderForegroundColor, Color ValueForegroundColor, Color BackgroundColor, bool SeparateRows = true, List<CellOptions>? CellOptions = null)
public static string RenderTablePlain(string[] Headers, string[,] Rows, int Margin, bool SeparateRows = true, List<CellOptions>? CellOptions = null)
public static string RenderTable(string[] Headers, string[,] Rows, int Margin, Color SeparatorForegroundColor, Color HeaderForegroundColor, Color ValueForegroundColor, Color BackgroundColor, bool SeparateRows = true, List<CellOptions>? CellOptions = null)
```
{% endcode %}

The implementation of the table introduced in the first version of Terminaux that was taken from Nitrocid 0.0.20 wasn't in any way good and nice, so we've decided to re-write the whole renderer from scratch. As a result, our tables look nicer and more beautiful.

However, we also had to change how you call the table rendering and writing functions so that they become easier to use.

{% hint style="info" %}
You can now place your headers at the top of the rows argument and set the `enableHeader` parameter to true. You'll have to determine the left, the top, the width, and the height of the table as it no longer depends on the current cursor position.

You'll have to take application behavior into account, as applications that depend on the previous behavior will need to be re-written.
{% endhint %}

### Terminaux.Extensions removal

{% code title="EnumerableExtensions.cs" lineNumbers="true" %}
```csharp
public static class EnumerableExtensions
```
{% endcode %}

As we don't want to cause conflicts of implementations and duplicate commits for each change in the enumerable extensions, we've decided to remove Terminaux.Extensions entirely and rely on Magico to provide such extensions.

{% hint style="info" %}
Migration to Terminaux 5.0 requires that you remove Terminaux.Extensions from your NuGet dependencies and replace it with Magico.
{% endhint %}
