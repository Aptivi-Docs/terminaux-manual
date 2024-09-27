---
description: Information in a box
---

# ℹ️ Informational Boxes

When you want to present information to the user in a form of a dialog box, you'll want to use this feature that presents you with a wide variety of informational box styles to choose from, depending on the purpose. Some informational box styles are modal, while others are non-modal. Modal informational boxes require input from the end user, while non-modal informational boxes don't require input and usually represent a progress being made.

The following informational box styles are presented in this way:

* Modal informational boxes
  * Buttons
  * Input
  * Password input
  * Modal dialog box
  * Slider
  * Selection
  * Multi selection
* Non-modal informational boxes
  * Non-modal dialog box
  * Progress

## Modal informational boxes

Informational box styles of this nature are modal. They require input from the end user in order to present information to the user with action. These boxes are common in interactive console applications where information is to be conveyed to the user. They are presented in a different style, based on how or what action the user needs to take, and their borders are customizable with the `BorderSettings` instance.

{% hint style="info" %}
Every single modal informational box style contain their own help page which you can access via the `K` key. They support both keyboard and mouse.
{% endhint %}

For informational boxes that use choice-based input styles, you can discover how to define choices by visiting this page:

{% content-ref url="../../input-reader/other-input/choice-based-inputs.md" %}
[choice-based-inputs.md](../../input-reader/other-input/choice-based-inputs.md)
{% endcontent-ref %}

### Buttons

This style utilizes choice-based input to render the buttons inside the informational box of this style, which makes them look like a conventional dialog box. This style can render up to three buttons from the right to the left, which means three instances of `InputChoiceInfo`. You can use this style with the `InfoBoxButtonsColor` class.

### Input

This style uses the input reader to tell the user to write something, based on the informational box contents. For example, if this informational box tells an end-user to write a hostname of an SFTP server, the user will have to write the hostname. You can use this style with `InfoBoxInputColor`.

### Input (password)

This style uses the input reader masked with the password to tell the user to write something, based on the informational box contents. For example, if this informational box tells an end-user to write a password of an account when logging in to the SFTP server, the user will have to write the password. You can use this style with `InfoBoxInputPasswordColor`.

### Normal modal info box

This style only prints information that the end user needs to read. This is suitable for information that doesn't need any action. You can use this style with the `InfoBoxModalColor` class.

### Slider

This style allows you to define a minimum, a current, and a maximum value of an integral value that can only be described as surrounded between the minimum value and the maximum value. For example, a slider that has a minimum value of 5 and a maximum value of 10 can only accept values of between 5 and 10, which means 5, 6, 7, 8, 9, and 10. You can use this style with the `InfoBoxSliderColor` class.

### Selection styles

There are two styles in this category:

* Single selection
* Multiple selection

#### Single selection informational box

This info box style uses the choice-based input to define choices that the end user will have to select one of them. Plus, you can search for a choice using the `F` key to initiate a regex-based search for maximum flexibility. You can use this style with the `InfoBoxSelectionColor` class.

#### Multiple selection informational box

This info box style uses the choice-based input to define multiple choices that the end user will have to select one of them. Plus, you can search for a choice using the `F` key to initiate a regex-based search for maximum flexibility. You can use this style with the `InfoBoxSelectionMultipleColor` class.
