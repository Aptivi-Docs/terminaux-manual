---
icon: gear
description: In case you want to set things up
---

# Reader Settings

The reader currently has a global settings instance session-wide. However, it can get overridden if you make a new instance of `TermReaderSettings` with some available properties mentioned below set. They get read each time you call this function or you activate a keybinding that uses one of these settings.

Here are the available settings that you can set:

* `PasswordMaskChar`
  * Sets the password masking character.
* `HistoryEnabled`
  * Whether the history is enabled or not.
* `HistoryName`
  * Select what history to use
* `LeftMargin`
  * Sets the left margin of the input.
* `RightMargin`
  * Sets the right margin of the input.
* `InputForegroundColor`
  * The foreground color for the input text, as well as the input prompt.
* `InputBackgroundColor`
  * The background color for the input text, as well as the input prompt.
* `Suggestions`
  * Sets a function that you could use to return an array of strings containing auto completion data.
* `SuggestionsDelimiters`
  * Delimiters to split the current input within a function that returns a list of suggestions.
* `TreatCtrlCAsInput`
  * Whether to treat `Ctrl` + `C` as input or not. If enabled, TermRead will process this keybinding as aborting the current input.
* `PlaceholderText`
  * This is the text that the terminal reader shows when there is no input text. This is used to give users hints on what they should write to the prompt.
* `PrintDefaultValue`
  * Whether to print the default value or not.
* `DefaultValueFormat`
  * The default value format to print if `PrintDefaultValue` is enabled and there is a prompt.
* `KeyboardCues`
  * Whether to play keyboard cues for each keypress
* `BassBoomLibraryPath`
  * In case of addons or other custom path for MPG123, specifies a root path to BassBoom's libraries. See [here](https://app.gitbook.com/o/fj052nYlsxW9IdL3bsZj/s/izAJoIbtQw1BdIlE4DBz/) for more info.
* `PlayWriteCue`
  * Specifies whether to play a write cue (that is, plays a sound for each keypress)
* `PlayRuboutCue`
  * Specifies whether to play a rubout cue (that is, plays a sound for every backspace press)
* `PlayEnterCue`
  * Specifies whether to play an enter cue (that is, plays a sound for each "submit" key, such as Enter)
* `CueVolume`
  * Specifies the cue volume in fractional number between `0.0` and `1.0` (or `3.0` with volume boost)
* `CueVolumeBoost`
  * Allows the cue volume to go above `1.0` to the maximum of `3.0`.

{% hint style="info" %}
You can copy the above settings in one line using the overload of the `TermReaderSettings` constructor that allows you to specify another `TermReaderSettings` instance to copy the settings from that instance to the new one. This is useful if you want to use the global settings without affecting future reads. This is a simple example of how to copy the global settings and to set the password mask without affecting the global password mask:

<pre class="language-csharp"><code class="lang-csharp">// Doesn't affect future reads
<strong>var settings = new TermReaderSettings(TermReader.GlobalReaderSettings)
</strong><strong>{
</strong><strong>    PasswordMaskChar = '#'
</strong><strong>};
</strong>
// Affects future reads
TermReader.GlobalReaderSettings.PasswordMaskChar = '#';
</code></pre>
{% endhint %}
