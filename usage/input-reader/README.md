---
description: May I read what you've written, please?
---

# 📖 Input Reader

This functionality is an important part of any interactive console application, because it gives users a chance to input what they want to write to the console.

In case you want to listen to mouse events, you can consult the below page:

{% content-ref url="pointer-events.md" %}
[pointer-events.md](pointer-events.md)
{% endcontent-ref %}

In case you want to use something other than the reader, you can consult the other input tools defined in the below page:

{% content-ref url="other-input/" %}
[other-input](other-input/)
{% endcontent-ref %}

You can easily use this feature in any interactive console application that uses Terminaux. Just use the `Terminaux.Reader.TermReader` class that contains the `Read()` functions and their overloaded versions.

{% hint style="info" %}
The reader not only provides the static text version for input prompts, but also the dynamic text version. Just create a simple function delegate that generates a string as the first argument, like this:

```csharp
string input = TermReader.Read(() => $"{DateTime.Now} [{TermReaderState.CurrentState.CurrentTextPos}]\n> ", "Hello World!", false, false, false);
```
{% endhint %}

{% hint style="info" %}
Please note that they are interruptible by default. If you want the input to be non-interruptible, you can set the `interruptible` argument to false.
{% endhint %}

Each one of these functions creates a reader state, `TermReaderState`, that contains essential information about the current reader state, including, but not limited to:

* Current text
* Input prompt text
* Current text position
* Kill buffer
* Reader settings

{% hint style="info" %}
If you're making your own mod in Nitrocid KS, it's best to use its own `Input` class instead of Terminaux's `TermReader`, as the class there actually deals with the screensaver in most circumstances.
{% endhint %}

Any key will append the selected characters to the current text input, and `RETURN` will accept the input. The below keybindings are available:

<table><thead><tr><th width="196.5">Keybinding</th><th>Action</th></tr></thead><tbody><tr><td><code>ENTER</code></td><td>Accepts input</td></tr><tr><td><code>Ctrl</code>+<code>C</code></td><td>Cancels reading (if <code>TreatCtrlCAsInput</code> is enabled)</td></tr><tr><td><code>Ctrl</code>+<code>A</code> / <code>HOME</code></td><td>Beginning of line</td></tr><tr><td><code>Ctrl</code>+<code>E</code> / <code>END</code></td><td>End of line</td></tr><tr><td><code>Ctrl</code>+<code>B</code> / <code>←</code></td><td>Backward one character</td></tr><tr><td><code>Ctrl</code>+<code>F</code> / <code>→</code></td><td>Forward one character</td></tr><tr><td><code>BACKSPACE</code></td><td>Remove one character from the left</td></tr><tr><td><code>UP ARROW</code></td><td>Get the older input</td></tr><tr><td><code>DOWN ARROW</code></td><td>Get the newer input</td></tr><tr><td><code>DELETE</code></td><td>Remove one character in current position</td></tr><tr><td><code>ALT</code>+<code>B</code></td><td>One word backward</td></tr><tr><td><code>ALT</code>+<code>F</code></td><td>One word forward</td></tr><tr><td><code>TAB</code></td><td>Next auto-completion entry (if there is one)</td></tr><tr><td></td><td>Insert four spaces (if no autocompletions)</td></tr><tr><td><code>SHIFT</code>+<code>TAB</code></td><td>Previous auto-completion entry</td></tr><tr><td><code>CTRL</code>+<code>U</code></td><td>Cut to the start of the line</td></tr><tr><td><code>CTRL</code>+<code>K</code></td><td>Cut to the end of the line</td></tr><tr><td><code>CTRL</code>+<code>W</code></td><td>Cut to the end of the previous word</td></tr><tr><td><code>ALT</code>+<code>D</code></td><td>Cut to the end of the next word</td></tr><tr><td><code>CTRL</code>+<code>Y</code></td><td>Yank the cut content</td></tr><tr><td><code>Alt</code>+<code>L</code></td><td>Make word lowercase</td></tr><tr><td><code>Alt</code>+<code>U</code></td><td>Make word UPPERCASE</td></tr><tr><td><code>Ctrl</code>+<code>Alt</code>+<code>L</code></td><td>Make input lowercase</td></tr><tr><td><code>Ctrl</code>+<code>Alt</code>+<code>U</code></td><td>Make input UPPERCASE</td></tr><tr><td><code>Alt</code>+<code>C</code></td><td>Make character uppercase and move to the end of word</td></tr><tr><td><code>Alt</code>+<code>V</code></td><td>Make character lowercase and move to the end of word</td></tr><tr><td><code>Alt</code>+<code>S</code></td><td>Shows all suggestions in the style akin to the Bourne Again SHell (bash)</td></tr><tr><td><code>Alt</code>+<code>R</code></td><td>Refreshes the prompt, the text input, and the current cursor position.</td></tr><tr><td><code>Insert</code></td><td>Text append mode (Insert or append)</td></tr><tr><td><code>CTRL</code>+<code>L</code></td><td>Clears the screen and refreshes the prompt.</td></tr><tr><td><code>ALT</code>+<code>\</code></td><td>Cut the whitespaces before and after the character.</td></tr><tr><td><code>CTRL</code>+<code>T</code></td><td>Substitutes two characters</td></tr><tr><td><code>ALT</code>+<code>T</code></td><td>Substitutes two words</td></tr><tr><td><code>ALT</code>+<code>SHIFT</code>+<code>#</code></td><td>Makes your current input text a comment (visual only, but ignores your text on submit)</td></tr><tr><td><code>ALT</code>+<code>TAB</code> / <code>CTRL</code>+<code>I</code></td><td>Forces the tab character to be written. Writes as spaces.</td></tr><tr><td><code>ALT</code>+<code>SHIFT</code>+<code>C</code></td><td>Temporarily conceals or reveals the whole input in normal prompts.</td></tr></tbody></table>

{% hint style="warning" %}
**Warning:** Some of the keys conflict with the terminal emulator and/or the operating system keybindings.
{% endhint %}

For more information about custom key bindings, go to the below page.

{% content-ref url="custom-bindings.md" %}
[custom-bindings.md](custom-bindings.md)
{% endcontent-ref %}

You can access the global reader settings by referencing the `GlobalReaderSettings` found in the `TermReader` class.

### History tools

You can now set the history entry list with your array of history entries or clear the history list using the following functions:

* `SetHistory(List<string> History)`
  * Sets the history to the chosen history list
* `ClearHistory()`
  * Clears all history entries

### State tools

You can also check to see if the console reader facility is busy getting input or not. The property, `Busy`, indicates this by returning `true` if there is input to be entered by the user.

{% hint style="info" %}
If you want to wait for user input to finish, you can call the `WaitForInput()` function in the `TermReaderTools` class.
{% endhint %}
