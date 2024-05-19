---
description: Breaking changes for API v4.0
---

# ⬆️ API v4.0

Here is a list of breaking changes that happened during the API v4.0 period when differing versions of Terminaux introduced breaking changes.

## From 3.4.x to 4.0.x

Between the 3.4.x and 4.0.x version range, we've made the following breaking changes:

### Removed enclosed versions of color sequence properties

{% code title="Color.cs" lineNumbers="true" %}
```csharp
public string PlainSequenceEnclosed
public string PlainSequenceEnclosedTrueColor
```
{% endcode %}

The plain sequences can already be enclosed in double quotes by escaping them in between the sequences, so these two properties are no longer considered useful. In earlier versions of Nitrocid, they were implemented as a workaround of handling `.ini` files that were responsible of handling Nitrocid 0.0.4 to 0.0.15 configuration.

{% hint style="info" %}
We advise you to manually enclose the `PlainSequence` and/or the `PlainSequenceTrueColor` properties with double quotes.
{% endhint %}

### Merged various VT sequence tools

{% code title="VtSequenceTools.cs" lineNumbers="true" %}
```csharp
public static string FilterVTSequencesMultiple(string Text, string replace = "", VtSequenceType types = VtSequenceType.All)
public static (VtSequenceType, Match[])[] MatchVTSequencesMultiple(string Text, VtSequenceType type = VtSequenceType.All)
public static bool IsMatchVTSequencesMultiple(string Text, VtSequenceType type = VtSequenceType.All)
```
{% endcode %}

We've made a refactor of various VT sequence tools in order to be more effective, so we've merged the `Single` and the `Multiple` versions together for the above functions in order to make them more efficient. Some of the tools remained unaffected.

{% hint style="info" %}
We advise you to change how you return the values as appropriate. It's almost exactly the same as the `Multiple` versions, but without the `Multiple` prefix.
{% endhint %}

### Removed non-standalone console writer wrappers

{% code title="ConsoleWrapperTools.cs" lineNumbers="true" %}
```csharp
public static Action<string, TermReaderSettings> ActionWriteStringNonStandalone
public static Action<string, TermReaderSettings, object[]> ActionWriteParameterizedNonStandalone
public static Action<string, TermReaderSettings> ActionWriteLineStringNonStandalone
public static Action<string, TermReaderSettings, object[]> ActionWriteLineParameterizedNonStandalone
```
{% endcode %}

We've finally removed the `TermReader`-exclusive console writer console wrappers as they were initially never meant to be overridden. It was due to Terminaux's reliance on internal functions in the non-standalone console writer wrapper. It was converted to a simple private function in the reader class.

{% hint style="danger" %}
We advice you to stop overriding the above four wrappers. If you rely on these wrappers, you should change your application code to make better use of Terminaux's tools to avoid having to override the standard wrappers for the terminal reader.
{% endhint %}

### Presentation system overhauled

{% code title="IElement.cs" lineNumbers="true" %}
```csharp
bool IsInput { get; }
string? WrittenInput { get; set; }
Action<object[]>? InvokeActionInput { get; }
Action? InvokeAction { get; }
```
{% endcode %}

{% code title="PresentationTools.cs" lineNumbers="true" %}
```csharp
public static int PresentationUpperBorderLeft
public static int PresentationUpperBorderTop
public static int PresentationUpperInnerBorderLeft
public static int PresentationUpperInnerBorderTop
public static int PresentationLowerInnerBorderLeft
public static int PresentationLowerInnerBorderTop
public static int PresentationInformationalTop
public static bool PresentationContainsInput(Slideshow presentation)
```
{% endcode %}

{% code title="Removed elements" lineNumbers="true" %}
```csharp
public class ChoiceInputElement : IElement
public class InputElement : IElement
public class MaskedInputElement : IElement
public class MultipleChoiceInputElement : IElement
```
{% endcode %}

We've overhauled the presentation system by changing how the input is processed. Now, instead of having the input represent an element, we've made a separate interface for them that simplifies the process of adding input to a presentation page and propagating data, called `IInputMethod`.

{% hint style="info" %}
Follow the instructions on how to add input to your presentation using [this page](../usage/console-tools/presentation-system.md).
{% endhint %}

### Presentation system now uses the screen feature fully

{% code title="IElement.cs" lineNumbers="true" %}
```csharp
void Render();
bool IsPossibleOutOfBounds();
```
{% endcode %}

As a result of the breakthroughs that we've done in Terminaux 4.0 compared to the previous series, we've decided to enhance the presentation system by making it fully rely on the screen feature to reduce the number of oddities and bugs that may occur and to reduce the complication and the feel of relying on the presentation dimensions to be able to do things as simple as printing a text.

This is done by removing the `IsPossibleOutOfBounds()` function and renaming `Render()` to `RenderToString()` to reflect its purpose.

Additionally, the presentation system has finally earned the ability to be scrolled as a result of this change!

{% hint style="info" %}
You should change how you're implementing your element to return a string that represents the element output, as Terminaux now handles all the positioning and the wrapping.
{% endhint %}

### `ClearPresentation()` removed

{% code title="PresentationTools.cs" lineNumbers="true" %}
```csharp
public static string ClearPresentation()
```
{% endcode %}

Terminaux now handles clearing the presentation automatically, so your elements can no longer call `ClearPresentation()`.

{% hint style="danger" %}
We advice you to cease using this function. In case of demand, we might bring it back.
{% endhint %}

### Simplified the console color data

{% code title="ConsoleColorData.cs" lineNumbers="true" %}
```csharp
public Rgb? RGB
public Hsl? HSL
public class Rgb
public class Hsl
```
{% endcode %}

The `Hsl` property and the class have been removed because it's unnecessary, given that Terminaux already provides the conversion tools. Meanwhile, we've changed the `RGB` property to return the Terminaux-managed `RedGreenBlue` class that you can use for many operations, such as conversion. We've also removed the `Rgb` decoy class.

This is to simplify the console color data by removing extraneous entries from the color data JSON file itself that Terminaux uses to fetch the color data.

{% hint style="info" %}
If you want to access the HSL info of a console color, you should convert the RGB instance that the `RGB` property returns using the available conversion tools pinpointed in [this page](../usage/color-sequences/color-model-conversions.md).
{% endhint %}

### Simplified the color blindness formula

{% code title="TransformationMethod.cs" lineNumbers="true" %}
```csharp
public enum TransformationMethod
```
{% endcode %}

We've decided to separate the color blindness formula class to the following:

* `BrettelColorBlind`
* `VienotColorBlind`

As a result, we've implemented three more enumerations clarifying that the three color blindness modes use the Vienot formula. Also, we've removed one argument, `method`, from the `RenderColorBlindnessAware()` function.

{% hint style="info" %}
To continue using the Vienot formula, you must use one of the following formulas as defined in the `TransformationFormula` enumeration:

* `ProtanVienot`
* `DeutanVienot`
* `TritanVienot`
{% endhint %}

### Updated Textify to migrate the Figletize library

We've merged the Figletize library with the Textify library so that Terminaux can provide more than 550+ Figlet fonts. As a result, we had to update Textify, which means that the Figletize library is no longer installed with Terminaux.

{% hint style="info" %}
You can consult the breaking change for Textify [here](https://app.gitbook.com/s/NaUWjRlaBR1k5rO42Zy8/breaking-changes#moved-figletize-to-textify.figlet).
{% endhint %}

### Removed unlimited input from the reader

{% code title="TermReaderSettings.cs" lineNumbers="true" %}
```csharp
public bool LimitConsoleChars
```
{% endcode %}

Unlimited input in the multi-line terminal reader was proven to be buggy, and we couldn't find a good solution to the problem, considering all the cases that need to be handled in the terminal reader. We've decided to remove unlimited input from the reader.

{% hint style="info" %}
If you still want unlimited input, you can use the one-line wrapped reader, which allows unlimited amount of characters.
{% endhint %}

### Slider width and height determination changes

{% code title="SliderColor.cs" lineNumbers="true" %}
```csharp
public static void WriteSliderPlain(int currPos, int maxPos, int Left, int Top, int WidthOffset, bool DrawBorder = true)
public static void WriteSliderPlain(int currPos, int maxPos, int Left, int Top, int LeftWidthOffset, int RightWidthOffset, bool DrawBorder = true)
public static void WriteSlider(int currPos, int maxPos, int Left, int Top, int WidthOffset, bool DrawBorder = true)
public static void WriteSlider(int currPos, int maxPos, int Left, int Top, int LeftWidthOffset, int RightWidthOffset, bool DrawBorder = true)
public static void WriteSlider(int currPos, int maxPos, int Left, int Top, int WidthOffset, Color SliderColor, bool DrawBorder = true)
public static void WriteSlider(int currPos, int maxPos, int Left, int Top, int LeftWidthOffset, int RightWidthOffset, Color SliderColor, bool DrawBorder = true)
public static void WriteSlider(int currPos, int maxPos, int Left, int Top, int WidthOffset, Color SliderColor, Color FrameColor, bool DrawBorder = true)
public static void WriteSlider(int currPos, int maxPos, int Left, int Top, int LeftWidthOffset, int RightWidthOffset, Color SliderColor, Color FrameColor, bool DrawBorder = true)
public static void WriteSlider(int currPos, int maxPos, int Left, int Top, int WidthOffset, Color SliderColor, Color FrameColor, Color BackgroundColor, bool DrawBorder = true)
public static void WriteSlider(int currPos, int maxPos, int Left, int Top, int LeftWidthOffset, int RightWidthOffset, Color SliderColor, Color FrameColor, Color BackgroundColor, bool DrawBorder = true)
public static string RenderSliderPlain(int currPos, int maxPos, int Left, int Top, int LeftWidthOffset, int RightWidthOffset, bool DrawBorder = true)
public static string RenderSlider(int currPos, int maxPos, int Left, int Top, int LeftWidthOffset, int RightWidthOffset, Color SliderColor, bool DrawBorder = true)
public static string RenderSlider(int currPos, int maxPos, int Left, int Top, int LeftWidthOffset, int RightWidthOffset, Color SliderColor, Color FrameColor, bool DrawBorder = true)
public static string RenderSlider(int currPos, int maxPos, int Left, int Top, int LeftWidthOffset, int RightWidthOffset, Color SliderColor, Color FrameColor, Color BackgroundColor, bool DrawBorder = true)
```
{% endcode %}

{% code title="SliderVerticalColor.cs" lineNumbers="true" %}
```csharp
public static void WriteVerticalSliderPlain(int currPos, int maxPos, int Left, int Top, int HeightOffset, bool DrawBorder = true)
public static void WriteVerticalSliderPlain(int currPos, int maxPos, int Left, int Top, int TopHeightOffset, int BottomHeightOffset, bool DrawBorder = true)
public static void WriteVerticalSlider(int currPos, int maxPos, int Left, int Top, int HeightOffset, bool DrawBorder = true)
public static void WriteVerticalSlider(int currPos, int maxPos, int Left, int Top, int TopHeightOffset, int BottomHeightOffset, bool DrawBorder = true)
public static void WriteVerticalSlider(int currPos, int maxPos, int Left, int Top, int HeightOffset, Color SliderColor, bool DrawBorder = true)
public static void WriteVerticalSlider(int currPos, int maxPos, int Left, int Top, int TopHeightOffset, int BottomHeightOffset, Color SliderColor, bool DrawBorder = true)
public static void WriteVerticalSlider(int currPos, int maxPos, int Left, int Top, int HeightOffset, Color SliderColor, Color FrameColor, bool DrawBorder = true)
public static void WriteVerticalSlider(int currPos, int maxPos, int Left, int Top, int TopHeightOffset, int BottomHeightOffset, Color SliderColor, Color FrameColor, bool DrawBorder = true)
public static void WriteVerticalSlider(int currPos, int maxPos, int Left, int Top, int HeightOffset, Color SliderColor, Color FrameColor, Color BackgroundColor, bool DrawBorder = true)
public static void WriteVerticalSlider(int currPos, int maxPos, int Left, int Top, int TopHeightOffset, int BottomHeightOffset, Color SliderColor, Color FrameColor, Color BackgroundColor, bool DrawBorder = true)
public static string RenderVerticalSliderPlain(int currPos, int maxPos, int Left, int Top, int TopHeightOffset, int BottomHeightOffset, bool DrawBorder = true)
public static string RenderVerticalSlider(int currPos, int maxPos, int Left, int Top, int TopHeightOffset, int BottomHeightOffset, Color SliderColor, bool DrawBorder = true)
public static string RenderVerticalSlider(int currPos, int maxPos, int Left, int Top, int TopHeightOffset, int BottomHeightOffset, Color SliderColor, Color FrameColor, bool DrawBorder = true)
public static string RenderVerticalSlider(int currPos, int maxPos, int Left, int Top, int TopHeightOffset, int BottomHeightOffset, Color SliderColor, Color FrameColor, Color BackgroundColor, bool DrawBorder = true)
```
{% endcode %}

Due to how difficult it was to determine the correct height from the left and the right offsets, we've decided to simplify things by replacing the offset system with the width/height system so that you can immediately determine the width and the height of your slider without having to perform additional calculations. This reduces the need of `ConsoleWrapper.Window{Width|Height}` call to determine the actual width/height from the two given offsets.

{% hint style="info" %}
We advise you to revise your slider calls and change them accordingly to represent the actual width/height.
{% endhint %}

### Merged three custom binding classes

{% code title="CustomBindings.cs" lineNumbers="true" %}
```csharp
public static class CustomBindings
```
{% endcode %}

The `CustomBindings` class has been recently expanded to include code from itself and the two internal classes - `BindingsReader` and `BindingsList` - that used to include both the binding execution tools and the list of pre-built and custom bindings. For easier maintenance, we've condensed these classes into one, renaming the `CustomBindings` class to `BindingsTools` to more accurately reflect its purpose.

{% hint style="info" %}
None of the functions are affected; just update your reference so that it points to `BindingsTools`.
{% endhint %}
