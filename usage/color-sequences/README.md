---
description: We need colors!
---

# 🎨 Color Sequences

Terminaux provides a functionality to generate color sequences with the help of the VT sequences. It allows you to generate a color with three modes:

* 16 colors
* 256 colors
* 16-bit colors (true color)

{% hint style="info" %}
Please note that some consoles don't support 256 colors and/or 16-bit colors, and some of them might implement these poorly (Terminal.app (`Apple_Terminal`) for example).

Support for 16-bit colors and 256-colors requires a compatible terminal emulator.

* Windows systems: ConEmu, Windows 10 cmd.exe with VT sequences enabled
* Linux systems: xterm, GNOME Terminal, Konsole, etc.
* macOS systems: iTerm2 only
{% endhint %}

This functionality contains several functions that you can make use of in your console application:

* Building a `Color` instance that supports RGB and 255-color modes
* Getting console color information from the 255-color mode
* Simulating color-blindness during compilation

More functionality is available in the right pane for conversion, parsing, and more.

## Building a `Color` instance

You can build your own `Color` instance for usage in your console application. There are various ways to build it:

```csharp
public Color(int R, int G, int B)
public Color(int R, int G, int B, ColorSettings settings)
public Color(ConsoleColors ColorDef)
public Color(ConsoleColors ColorDef, ColorSettings settings)
public Color(ConsoleColor ColorDef)
public Color(ConsoleColor ColorDef, ColorSettings settings)
public Color(int ColorNum)
public Color(int ColorNum, ColorSettings settings)
public Color(string ColorSpecifier)
public Color(string ColorSpecifier, ColorSettings settings)
```

The `ColorSpecifier` can be of the syntax:

* `<num>`
  * `<num>` should be of the range between 0 and 255
* `<rrr>;<ggg>;<bbb>`
  * `<rrr>`, `<ggg>`, and `<bbb>` should be of the range between 0 and 255
* `cmyk:<ccc>;<mmm>;<yyy>;<kkk>`
  * `<ccc>`, `<mmm>`, `<yyy>`, and `<kkk>` should be of the range between 0 and 100
* `cmy:<ccc>;<mmm>;<yyy>`
  * `<ccc>`, `<mmm>`, and `<yyy>` should be of the range between 0 and 100
* `hsl:<hhh>;<sss>;<lll>`
  * `<hhh>` should be of the range between 0 and 360 in degrees and not radians
  * `<sss>` and `<lll>` should be of the range between 0 and 100
* `hsv:<hhh>;<sss>;<vvv>`
  * `<hhh>` should be of the range between 0 and 360 in degrees and not radians
  * `<sss>` and `<vvv>` should be of the range between 0 and 100
* `ryb:<rrr>;<yyy>;<bbb>`
  * `<rrr>`, `<yyy>`, and `<bbb>` should be of the range between 0 and 255, just like RGB.
* `#000000`
  * Hexadecimal representation of the color for HTML fans. You can also use the `#RGB` format, implying that the three digits represent:
    * R: Red color level converted to RR (F becomes FF)
    * G: Green color level converted to GG (F becomes FF)
    * B: Blue color level converted to BB (F becomes FF)
* `<ColorName>`
  * Color name from `ConsoleColors` enumeration

{% hint style="info" %}
You can also specify just a string, an integer, a `ConsoleColor`, or a `ConsoleColor` enumeration value when making a variable that holds the `Color` class like the following:

```csharp
Color ColorInstance = 18;
Color ColorInstance = "94;0;63";
Color ColorInstance = "#0F0F0F";
Color ColorInstance = "#F0F";
Color ColorInstance = "Magenta";
Color ColorInstance = ConsoleColors.Magenta;
Color ColorInstance = ConsoleColor.Magenta;
```
{% endhint %}

You can also get the color ID if you used color number from 0 to 255 by referencing the `ColorId` property. This property returns `-1` for true color.

Additionally, you can choose whether to use your terminal emulator's color palette or to use the real colors that come from the true colors. By default, Terminaux chooses to use the terminal emulator's color palette to maintain consistency.

{% hint style="info" %}
In addition to the `PlainSequence` property, you can also use the `ToString()` function to get the same value from that property. This allows easier string interpolation, such as:

```csharp
BoxFrameTextColor.WriteBoxFrame($"Red, Green, and Blue: {selectedColor}", hueBarX, rgbRampBarY, boxWidth, boxHeight + 2);
```
{% endhint %}

## Determining color brightness

You can check to see if a color is a light or a dark color using the `Brightness` property, which returns either of the following:

* `Dark`
* `Light`

There is a usage of this feature to determine the appropriate gray color, which contains a function found in the same class.

### Getting a gray color

You can get a gray color either for the current background color or for your custom background color using the `GetGray()` function. This allows you to easily get an appropriate color if you want to write something in it and you don't know what color to choose.

{% code title="ColorTools.cs" lineNumbers="true" %}
```csharp
public static Color GetGray()
public static Color GetGray(Color color)
```
{% endcode %}

## Getting console color information

You can get detailed information about the console color ranging from 0 to 255 by making a new instance of the `ConsoleColorsInfo` class:

```csharp
public ConsoleColorsInfo(ConsoleColors ColorValue)
```

You can then get its `Color` instance using the `Color` property.

## Simulating color-blindness

In the `ColorTools` static class, it contains several color blindness simulation tools that you can use:

* `EnableColorTransformation`
  * Enables the color transformation to adjust to color blindness upon making a new instance of color
* `ColorTransformationMethod`
  * Chooses the color transformation method. This only applies to some of the color-blindness transformations, such as Protan.
* `ColorTransformationFormula`
  * Specifies the type of color transformation (Protan, Deutan, Tritan, Monochromacy, Inverse, and more...)
* `ColorDeficiencySeverity`
  * Specifies the severity of the color deficiency ranging between 0.0 and 1.0 from lowest to highest

{% hint style="info" %}
Both the severity and the transformation formula flags don't affect the monochromacy color transformer.
{% endhint %}

After you change these values, the next time you make a new instance of `Color`, you'll notice that the resulting color is shifted to adjust to color-blindness.

{% hint style="info" %}
You can easily make a new `Color` instance using the brand new API function, `RenderColorBlindnessAware()`.
{% endhint %}

### Translate from X11 to ConsoleColor and back

`ConsoleColor` enumeration has an order of colors that is slightly different from the X11 colormap definitions for the first 16 colors. The following colors differ from each other:

| ConsoleColor              | X11                        |
| ------------------------- | -------------------------- |
| `ConsoleColor.DarkBlue`   | `ConsoleColors.DarkRed`    |
| `ConsoleColor.DarkCyan`   | `ConsoleColors.DarkYellow` |
| `ConsoleColor.DarkRed`    | `ConsoleColors.DarkBlue`   |
| `ConsoleColor.DarkYellow` | `ConsoleColors.DarkCyan`   |
| `ConsoleColor.Blue`       | `ConsoleColors.Red`        |
| `ConsoleColor.Cyan`       | `ConsoleColors.Yellow`     |
| `ConsoleColor.Red`        | `ConsoleColors.Blue`       |
| `ConsoleColor.Yellow`     | `ConsoleColors.Cyan`       |

In the `ColorTools` class, you can find two functions that translate between the two color mappings:

*   If you want to translate from `ConsoleColor` to X11's representation (`ConsoleColors`), use the `TranslateToX11ColorMap()` function, provided that its signature is:\


    ```csharp
    public static ConsoleColors TranslateToX11ColorMap(ConsoleColor color)
    ```
*   If you want to translate from `ConsoleColors` to .NET's representation (`ConsoleColor`), use the `TranslateToStandardColorMap()` function, provided that its signature is:\


    ```csharp
    public static ConsoleColor TranslateToStandardColorMap(ConsoleColors color)
    ```

### Correcting the ConsoleColor map for X11

Terminaux also provides a function that gets the correct color mapping for the specified color. The function is found under `ColorTools`, called `CorrectStandardColor()`. The signature is defined below:

```csharp
public static ConsoleColor CorrectStandardColor(ConsoleColor color)
```

### Resetting the colors and the console

Terminaux provides a console extension that allows you to perform a hard reset using the two VT sequences. When invoked, the terminal will perform two resets:

* Full reset (ESC sequence)
* Soft reset (CSI sequence)

After the reset is done, the screen will be cleared with all the colors reverted to their initial state. The signature is defined below:

```csharp
public static void ResetAll()
```

#### Resetting colors

You can reset all the colors once you're done writing text with color using the `ResetColors()` method. This resets the foreground and the background colors without resetting the whole console.

{% code title="ConsoleExtensions.cs" lineNumbers="true" %}
```csharp
public static void ResetColors()
```
{% endcode %}

If you want to reset only the foreground or the background color, you can use the following two functions:

{% code title="ConsoleExtensions.cs" lineNumbers="true" %}
```csharp
public static void ResetForeground()
public static void ResetBackground()
```
{% endcode %}

### Using settings

You can also generate colors with your specific settings, such as color transformation formula, by making a new instance of `ColorSettings` to store your own color settings.

However, by passing in your new instance of `ColorSettings`, you're affecting this color generation only once. To make it affect all generations, you'll have to change the global color settings, which can be found on `ColorTools.GlobalSettings`.

{% hint style="info" %}
Any change to a value in the global settings affects all color generations. Use your instance of `ColorSettings`, if possible.
{% endhint %}

You can also use your `ColorSettings` instance when parsing your color specifier.
