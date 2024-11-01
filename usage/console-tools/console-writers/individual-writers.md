---
icon: pencil-mechanical
description: Here's the documentation for individual writers
---

# Individual Writers

## Standard console writers

The standard console writers provide you with non-moving parts of the text. They have their own writers and renderers to allow you to use them in two ways. The writer writes directly to the console, while the renderers are more suitable for the screen feature that you can check out at:

{% content-ref url="../console-screen.md" %}
[console-screen.md](../console-screen.md)
{% endcontent-ref %}

### Normal console writers

Starting from `ConsoleWriters`, this namespace provides the following classes:

* `ListEntryWriterColor`
  * Provides you with the necessary functions to let you write a list entry to the console easily.
* `ListWriterColor`
  * Provides you with the necessary functions to let you write the list entries to the console easily. It supports both non-generic and generic enumerables and dictionaries.
* `TextWriterRaw`
  * Provides you with the necessary functions to allow you to write the raw text to the console either to the standard output or the standard error.
* `TextWriterColor`
  * Provides you with the necessary functions to allow you to write the text to the console with and without color.
* `TextWriterHighlightedColor`
  * Provides you with the necessary functions to allow you to write the highlighted text to the console with and without color.
  * By default, the modern way of highlighting specific text is enabled by using a VT sequence intended to reverse the colors. If you still want to use the legacy way, you'll have to set the `legacy` argument to `true` before passing in all the usual parameters.
* `TextWriterWhereColor`
  * Provides you with the necessary functions to write the text in a specific position to the console with and without color.

Consult the below page to find out how to use these functions.

{% embed url="https://aptivi.github.io/Terminaux/api/Terminaux.Writer.ConsoleWriters.html" %}

{% hint style="info" %}
The console writers have three variants of writing functions:

* `Write()`: Default colors
* `WriteColor()`: Foreground colors
* `WriteColorBack()`: Foreground and background colors
{% endhint %}

#### List entry writer (`ListEntryWriterColor`)

This writer allows you to easily write one list entry with its value to the console without having to use two different functions or a long function call for writing this entry. This is internally used by the ListWriterColor writer, which is shown below.

#### List writer (`ListWriterColor`)

This writer allows you to easily write list entries from either an array or an enumerable, such as lists and dictionaries, to the console without having to write loop statements. This is available in both the generic and the non-generic versions.

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

#### Normal text writer (`TextWriterColor`)

This writer is the simplest writer with color support, and can be used to write general text to the console effortlessly.

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Highlighted text writer (`TextWriterHighlightedColor`)

This writer takes the colors and reverts the background and the foreground colors to make text look like as if it's highlighted.

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

#### Raw text writer (`TextWriterRaw`)

This writer provides access to plain console writers and raw console writers that allow you to write text plainly. As for the raw console writing function, it's found in the same class that you can use to print raw text to the console, including escape sequences.

#### Positional text writer (`TextWriterWhereColor`)

This text writer allows you to write text at any position to the console. This moves the cursor to the destination, writes text to the console, and, if instructed, to return to the original position that the cursor was on before the writing process.

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### Fancy writers

Alongside these writers, there are also writers that are categorized as "fancy" because they either print so awesome or they print graphics. Some of these writers allow you to supply text and/or a title. They can be found in the `FancyWriters` namespace that provides the below classes:

* `AlignedFigletTextColor`
  * Provides you with the necessary functions to allow you to render a string using the provided Figlet font either at the left of the console, at the middle of the console, or at the right of the console.
* `AlignedTextColor`
  * Provides you with the necessary functions to allow you to render a string either at the left of the console, at the middle of the console, or at the right of the console.
* `BorderColor`
  * Provides you with the necessary functions to allow you to draw a border somewhere in the console and with colors (border, background, and text).
* `BorderTextColor`
  * Provides you with the necessary functions to allow you to draw a border somewhere in the console with text inside the box with support for colors (border, background, and text) and alignment.
* `BoxColor`
  * Provides you with the necessary functions to allow you to draw a box somewhere in the console.
* `BoxFrameColor`
  * Provides you with the necessary functions to allow you to draw a box frame somewhere in the console with support for colors (border, background, and text), text alignment, and title alignment.
* `FigletColor`
  * Provides you with the necessary functions to allow you to render a string using the provided Figlet font to the console.
* `FigletWhereColor`
  * Provides you with the necessary functions to allow you to render a string using the provided Figlet font to the console at any position you want.
* `InfoBoxColor`
  * Provides you with the necessary functions to allow you to render an information box containing text inside it in the middle of the console. You can make it either modal (having a user press any key to exit) or informational (not waiting for any user input).
* `PowerLineColor`
  * Provides you with the necessary functions to allow you to build PowerLine segments and display them to the console.
* `ProgressBarColor`
  * Provides you with the necessary functions to allow you to render a horizontal progress bar to the console.
* `ProgressBarVerticalColor`
  * Provides you with the necessary functions to allow you to render a vertical progress bar to the console.
* `RainbowTextWriterColor`
  * Provides you with the necessary functions to write the text with a rainbow colors spread around the text as foreground color.
* `RainbowBackTextWriterColor`
  * Provides you with the necessary functions to write the text with a rainbow colors spread around the text as background color.
* `SeparatorWriterColor`
  * Provides you with the necessary functions to allow you to render a separator including text to the console.
* `TableColor`
  * Provides you with the necessary functions to allow you to render a table to the console with alignment and color support.
* `SliderVerticalColor`
  * Provides you with the necessary functions to allow you to render a vertical slider to the console. The `Absolute` versions of the functions are generally easier to use than the normal ones, because they describe ranges as actual numbers rather than the slider console cells.
* `SliderColor`
  * Provides you with the necessary functions to allow you to render a horizontal slider to the console. The `Absolute` versions of the functions are generally easier to use than the normal ones, because they describe ranges as actual numbers rather than the slider console cells.
* `TruncatedText`
  * Provides you with renderers that allow you to render a truncated text block that's limited according to the height and the width, with the current column and row position of the text.

Consult the below page to find out how to use these functions.

{% embed url="https://aptivi.github.io/Terminaux/api/Terminaux.Writer.FancyWriters.html" %}

The tools for fancy writers can also be found here:

{% embed url="https://aptivi.github.io/Terminaux/api/Terminaux.Writer.FancyWriters.Tools.html" %}

{% hint style="info" %}
In order to use text alignment and other settings, you must first create an instance of `TextSettings`. You can use the `TextWriterTools` to gain access to low-level tools for your writing methods, such as if you're creating a custom writer tailored for your app.

There are also cyclic writers available for some of the fancy writers.
{% endhint %}

#### Aligned Figlet text color writer (`AlignedFigletTextColor`)

This writer allows you to write a Figlet text with alignment and color support, such as left alignment, center alignment, and right alignment.

{% tabs %}
{% tab title="Left alignment" %}
<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Center alignment" %}
<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Right alignment" %}
<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

#### Aligned text color writer (`AlignedTextColor`)

This text writer allows you to write an aligned text with color support. It wraps text to either the left, the center, or to the right, similar to how word processors work.

{% tabs %}
{% tab title="Left alignment" %}
<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Center alignment" %}
<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Right alignment" %}
<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

#### Border color writer (`BorderColor`)

This writer allows you to write a customizable border anywhere on the console. You can customize the border style, the background, and the title and its settings, such as alignment. This uses `BorderTextColor` to write a border, which you can refer to below.

#### Border with text color writer (`BorderTextColor`)

This writer allows you to write a customizable border anywhere on the console with text being printed inside the border. You can customize the border style, the background, and the title and its settings, such as title and text alignment.

<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

#### Box color writer (`BoxColor`)

This writer allows you to write a rectangle to the console in any position.

#### Box frame color writer (`BoxFrameColor`)

This writer allows you to write a box border to the console in any position with text title support, color customization, and border customization.

#### Figlet color writer (`FigletColor`)

This provides you with an easy way to write a Figlet text to the console with color, margin, and formatting support.

<figure><img src="../../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

#### Figlet color positional writer (`FigletWhereColor`)

This provides you with an easy way to write a Figlet text anywhere in the console with color, margin, and formatting support.

#### PowerLine writer (`PowerLineWriter`)

This writer allows you to form PowerLine elements and write them to the console. You can create a `PowerLineSegment` list that provides the following properties:

* Segment foreground: The `SegmentForeground` property determines the text color of a segment.
* Segment background: The `SegmentBackground` property determines the background color of a PowerLine segment.
* Segment icon: An iconic character determined by the `SegmentIcon` property that contains a segment symbol for better readability. You can use either simple icons or a Nerd Fonts icon that you can consult [here](../nerd-fonts.md).
* Segment transition icon: An iconic character that contains a segment transition icon. This is determined by the `SegmentTransitionIcon` property and should have a value of a PowerLine transition icon.
* Segment text: Text of a segment that will be shown. It's usually filled with status information. This is determined by the `SegmentText` property.

<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

#### Progress bar color (`ProgressBarColor`)

This writer allows you to write a static horizontal progress bar for determinate progresses. This supports color, customized borders, and positioning. This is internally used by the informational box writer for progress bars.

#### Vertical progress bar color (`ProgressBarVerticalColor`)

This writer allows you to write a static vertical progress bar for determinate progresses, such as equalizers. This supports color, customized borders, and positioning.

#### Rainbow text writer (`RainbowTextWriterColor`)

This writer allows you to write a rainbow-colored text to the console with line and background color support.

<figure><img src="../../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

#### Background rainbow text writer (`RainbowBackTextWriterColor`)

This writer allows you to write a text with rainbow color background to the console with line and background color support.

<figure><img src="../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

#### Separator writer (`SeparatorWriterColor`)

This text writer allows you to easily write a separator that can be used to easily separate sections. This supports custom colors.

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

#### Slider writer (`SliderColor`)

This writer allows you to write a horizontal slider to the console in any position and width with color and customized border support.

#### Vertical slider writer (`SliderVerticalColor`)

This writer allows you to write a vertical slider to the console in any position and height with color and customized border support.

#### Table writer (`TableColor`)

This writer allows you to easily write a customizable table with support for colors, customized borders, customized cells, header row, and more. You can specify the cell options for your table by providing a list of `CellOptions` instances. They provide the following properties:

* `ColumnNumber`: A one-based column number as specified by the user, and you can access its zero-based index using the `ColumnIndex` property.
* `RowNumber`: A one-based row number as specified by the user, and you can access its zero-based index using the `RowIndex` property.
* `ColoredCell`: Whether to highlight the cell with customized foreground and background colors. If it's enabled, the table writer will use the following properties to apply the colors:
  * `CellColor`: Chooses the foreground color of the cell.
  * `CellBackgroundColor`: Chooses the background color of the cell.
* `TextSettings`: Provides you a way to customize how text in the cells is printed, including alignment.

<figure><img src="../../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

#### Truncated text writer (`TruncatedText`)

This writer allows you to render a text block with the height and the width of your choice, taking the current row and column into account to decide what part of text is printed.

### Miscellaneous writers

Finally, the miscellaneous writers are the writers that don't have any meaningful category. That's when `MiscWriters` comes in. This namespace contains these classes:

* `LineHandleWriter`
  * Provides you with the necessary functions to allow you to render a line of a text file with the compiler-like line handle using the specified line and column to the console.
* `LineHandleRangedWriter`
  * Provides you with the necessary functions to allow you to render a line of a text file with the compiler-like line handle using the specified line and column range to the console. This is used to highlight relevant parts of the entire line
* `KeybindingsWriter`
  * Provides you with the necessary functions that allow you to write the horizontal list of keybindings according to the list specified. Some of these functions also allow you to write a modal informational box that shows a list of keybindings. For more information, see [this page](../../input-reader/other-input/keybindings.md).

Consult the below page to find out how to use these functions:

{% embed url="https://aptivi.github.io/Terminaux/api/Terminaux.Writer.MiscWriters.html" %}

#### Line handle writer (`LineHandleWriter` and `LineHandleRangedWriter`)

These writers allow you to write a line from a file or an array of lines with the line number and a handle that points to either a specified column number or a range. This is similar to the GCC compiler's method of showing you the errors and warnings, but it's implemented in a more general way.

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

#### Keybinding list writer (`KeybindingsWriter`)

This writer allows you to easily write a list of keybindings to the console effortlessly. It supports margins, positions, customized colors, and customized "help" key. It also provides functions that allow you to write a list of keybindings for informational boxes. This is used by most interactive applications, including the interactive TUI.

<figure><img src="../../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

## Dynamic writers

In addition to the standard writers, there exists dynamic writers that allow you to render moving parts of text to the console. This type of writers are usable only when writing to the console directly.

This namespace contains these classes:

* `TextWriterSlowColor`
  * Provides you with the necessary functions to simulate a typewriter writing a requested string to the console with and without color.
* `TextWriterWhereSlowColor`
  * Provides you with the necessary functions to simulate a typewriter that writes a text in a specific position to the console with and without color.
* `TextWriterWrappedColor`
  * Provides you with the necessary functions to allow you to wrap long outputs to pages, also called a pager.

Consult the below page to find out how to use these functions:

{% embed url="https://aptivi.github.io/Terminaux/api/Terminaux.Writer.DynamicWriters.html" %}

## Miscellaneous

Some of the console writers allow customization, such as the bordered boxes or progress bars.

### Borders

You can customize the borders for some of the console writers that support borders by editing properties found in the global settings of the borders, which can be found in the `BorderSettings.GlobalSettings` property. You can pass your own instance of `BorderSettings` with your own customizations to the border and its properties to the supported writers and all the informational boxes. The following writers support editing the border settings:

* `BorderColor`
* `BorderTextColor`
* `BoxFrameColor`
* `ProgressBarColor`
* `ProgressBarVerticalColor`
* `SliderColor`
* `SliderVerticalColor`
* ...and much more

{% hint style="info" %}
Pre-defined borders can be accessed by calling one of the following properties in the `PredefinedBorders` class:

* `Default`
* `RectangleTwoLines`
* `RectangleTwoLinesEdge`
* `RectangleThin`
* `RectangleThick`
* `RectangleThickEdge`
* `RectangleSimple`
* `HorizontalIntersection`
* `VerticalIntersection`
* `Intersections`
* `HorizontalIntersectionThick`
* `VerticalIntersectionThick`
* `IntersectionsThick`
* `HorizontalIntersectionDouble`
* `VerticalIntersectionDouble`
* `IntersectionsDouble`
{% endhint %}

### Wrapped pager controls

The below wrapped pager controls are available when wrapping is enabled:

* `ESC`: Exits the pager
* `Page Up`: Moves the output by one page backward, but stops at the beginning of the output.
* `Page Down`: Moves the output by one page forward, but stops at the end of the output.
* `Up Arrow`: Moves up by one line
* `Down Arrow`: Moves down by one line
* `Home`: Goes to the first page
* `End`: Goes to the last page
* `Any key`: Moves the output by one page forward and exits if it reaches end of line
