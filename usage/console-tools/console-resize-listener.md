---
description: We're listening for any size changes!
---

# ☎ Console Resize Listener

Terminaux also provides you with a polling-based console resize listener that allows your console application, especially the interactive ones, to listen to every single console resize event for all the platforms. It works on Windows, macOS, Linux, and Android.

{% hint style="info" %}
This listener can be started upon request with `StartResizeListener()`, but can't be stopped until the application is exited either gracefully or ungracefully.
{% endhint %}

The console resize listener contains several of the convenience functions that allow you to control the listener.

* `WasResized()`: This function tells you if the console has been resized before or not. If it has been resized before, it returns `true` and sets the cached resized state to `false`.
* `GetCurrentConsoleSize()`: This function gives you a tuple that represents the cached window width and the window height.

{% hint style="info" %}
Upon calling `WasResized()`, it sets the cached resized state to false. You need to either store this value to a local variable or you need to pass `false` to the function to tell it not to reset the state.

The second function gets the console size from the terminal if the listener hasn't started yet. Otherwise, it returns the cached window width and the height in case it's run on a loop, directly or indirectly.
{% endhint %}
