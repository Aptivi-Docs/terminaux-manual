---
description: Other input...
---

# 🖱️ Other Input

Terminaux not only provides the normal input reader with its custom bindings and its customizability, but you can also use its other input functions, such as informational boxes.

Terminaux currently provides the following input styles:

* Choice (one line, two lines, modern, and table)
* Info box (buttons, normal, input, password input, progress, selection, and multiple choices)
* Selection (single choice, multiple choices)

In addition to the three styles, you can also consult the following additional and specialized styles:

{% content-ref url="figlet-font-selector.md" %}
[figlet-font-selector.md](figlet-font-selector.md)
{% endcontent-ref %}

{% content-ref url="color-wheel.md" %}
[color-wheel.md](color-wheel.md)
{% endcontent-ref %}

The selection style contains a settings argument that you can easily make a class instance out of it, called `SelectionStyleSettings`. This class contains a singleton property that serves as a global settings for the selection input settings. This class currently contains the following color settings:

* Question
* Slider
* Input
* Option
* Alternative Option
* Selected Option
* Disabled Option
* Separator
* Text

{% hint style="info" %}
The overloads that contain a settings argument let you pass your custom settings to the current selection style invocation, while the overloads that don't contain this parameter use the global settings.
{% endhint %}

All input choices use the `InputChoiceInfo` array to define the choices. Not only do they indicate choices, but they also contain some settings, such as the default selection position, default selection indicator (like a checkbox, checked or not), and disabled option.

* `ChoiceDefault`: Indicates whether the choice instance is a default choice or not. All input handlers should select the first instance with this variable set to true as a default choice. Disabled choices can't be a default choice at the same time.
* `ChoiceDefaultSelected`: Indicates whether this choice instance is ticked or not (as in multiple selection, such as checkboxes). Disabled choices can set this as enabled.
* `ChoiceDisabled`: Indicates whether the user can not interact with this choice (select or tick it).

{% hint style="danger" %}
All input must require that at least one of the choices is enabled by default. Otherwise, an exception will be thrown.
{% endhint %}

If you want to obtain the `InputChoiceInfo` array, you can use the `InputChoiceTools` class to be able to use either a set of answers as a single string delimited by the slash character, such as Y/N/C, or an array of answers. Their titles can be optionally defined, but they will be numbered if either the answers or the titles are missing.

Below functions will help you get an array of `InputChoiceInfo`:

* `GetInputChoices(string AnswersStr, string[] AnswersTitles)`
* `GetInputChoices(string[] Answers, string[] AnswersTitles)`

{% hint style="info" %}
You can use this output to define choices while calling the above input functions.
{% endhint %}
