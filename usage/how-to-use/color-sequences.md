---
description: We need colors!
---

# 🎨 Color Sequences

{% hint style="info" %}
This feature is available on 1.0.0 or higher.
{% endhint %}

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

## Building a `Color` instance

You can build your own `Color` instance for usage in your console application. There are various ways to build it:

```csharp
public Color(int R, int G, int B)
public Color(ConsoleColors ColorDef)
public Color(ConsoleColor ColorDef)
public Color(int ColorNum)
public Color(string ColorSpecifier)
```

The `ColorSpecifier` can be of the syntax:

* `<num>`
  * `<num>` should be of the range between 0 and 255
* `<rrr>;<ggg>;<bbb>`
  * `<rrr>`, `<ggg>`, and `<bbb>` should be of the range between 0 and 255
* `#000000`
  * Hexadecimal representation of the color for HTML fans
* `<ColorName>`
  * Color name from `ConsoleColors` enumeration

{% hint style="info" %}
You can also specify just a string, an integer, a ConsoleColor, or a ConsoleColor enumeration value when making a variable that holds the Color class like the following:

```csharp
Color ColorInstance = 18;
Color ColorInstance = "94;0;63";
Color ColorInstance = "#0F0F0F";
Color ColorInstance = "Magenta";
Color ColorInstance = ConsoleColors.Magenta;
Color ColorInstance = ConsoleColor.Magenta;
```
{% endhint %}

## Getting console color information

You can get detailed information about the console color ranging from 0 to 255 by making a new instance of the `ConsoleColorsInfo` class:

```csharp
public ConsoleColorsInfo(ConsoleColors ColorValue)
```

## Simulating color-blindness

{% hint style="info" %}
This feature is not available in the Rust port of ColorSeq.
{% endhint %}

In the `ColorTools` static class, it contains several color blindness simulation tools that you can use:

* `EnableColorTransformation`
  * Enables the color transformation to adjust to color blindness upon making a new instance of color
* `EnableSimpleColorTransformation`
  * Enables the simple color transformation. This changes formula from Brettel 1997 (value is false) to Vienot 1999 (value is true)
* `ColorDeficiency`
  * Specifies the type of color blindness (Protan, Deutan, and Tritan)
* `ColorDeficiencySeverity`
  * Specifies the severity of the color deficiency ranging between 0.0 and 1.0 from lowest to highest

After you change these values, the next time you make a new instance of `Color`, you'll notice that the resulting color is shifted to adjust to color-blindness.

### Translate from X11 to ConsoleColor and back

ConsoleColor enumeration has an order of colors that is slightly different from the X11 colormap definitions for the first 16 colors. The following colors differ from each other:

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

Since 1.3.0, Terminaux provides a console extension that allows you to perform a hard reset using the two VT sequences. When invoked, the terminal will perform two resets:

* Full reset (ESC sequence)
* Soft reset (CSI sequence)

After the reset is done, the screen will be cleared with all the colors reverted to their initial state. The signature is defined below:

```csharp
public static void ResetAll()
```

### Conversions of Color Models

In addition to Terminaux supporting RGB color model, you can also use the CMYK and HSL color models when creating the color instances, provided that their specifiers that you must use are:

* CMYK's specifier is `cmyk:ccc;mmm;yyy;kkk`
* HSL's specifier is `hsl:hhh;sss;lll`

For CMYK specifiers, all the variables are from 0 to 100

For HSL specifiers, the `h` variable is from 0 to 360 degrees (not radians) and the `s` and the `l` variables are from 0 to 100.

To convert from RGB to CMYK or HSL, you need an instance of the `Color` class first with the color of your choice. Afterwards, you can use the `RGB` property of the `Color` class to obtain conversion functions, which are:

* `ConvertToCmyk()`
* `ConvertToHsl()`

The conversion is done to their separate classes that are appropriate for the following color models:

#### CMYK (Cyan, Magenta, Yellow, and Black Key)

This color model contains two separate classes: the Cyan Magenta Yellow, and the wrapper class with the black key value.

* `CyanMagentaYellow`
  * This is the class that stores the Cyan, the Magenta, and the Yellow values in both the fractional and the whole formats.
* `CyanMagentaYellowKey`
  * This is the class that is a wrapper of the `CyanMagentaYellow` class along with the black key in both the fractional and the whole formats.

To convert from CMYK to RGB or HSL, you can use the following functions:

* `ConvertToRgb()`
* `ConvertToHsl()`

#### HSL (Hue, Saturation, and Lumiance (Lightness))

This color model contains a single color model class that contains the necessary variables to represent the color hue, the color saturation, and the color lumiance (or lightness).

To convert from HSL to RGB or CMYK, you can use the following functions:

* `ConvertToRgb()`
* `ConvertToCmyk()`
