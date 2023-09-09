---
description: We have breaking changes here...
---

# 🥛 Breaking changes

As new Terminaux versions are being made, we've documented all the breaking changes when upgrading to newer versions of Terminaux. Here are the list of all the breaking changes that happened, starting from the oldest version to the latest.

## From 1.0.x to 1.1.x

Between the 1.0.x and 1.1.x version range, we've made the following breaking changes:

### Moved `FigletTools`

{% code title="FigletTools.cs" lineNumbers="true" %}
```csharp
public static class FigletTools
```
{% endcode %}

In preparation for the figlet font selector, we've decided to move this class from the `Terminaux.Writer.FancyWriters.Tools` namespace to a more appropriate place, which is `Terminaux.Figlet`.&#x20;

{% hint style="info" %}
We advice you to add a new `usings` clause in the beginning of the code file that uses `FigletTools` so that it points to the new namespace, like below:

```csharp
using Terminaux.Figlet
```
{% endhint %}

## From 1.1.x to 1.4.x

Between the 1.1.x and 1.4.x version range, we've made the following breaking changes:

### Added the signing key (a.k.a. strong name) <a href="#added-the-signing-key-a.k.a.-strong-name" id="added-the-signing-key-a.k.a.-strong-name"></a>

Nitrocid KS launched without any signing key. Originally, it was planned to come signed by us, but it didn't work. However, we used the strong naming tool to give Nitrocid KS a unique identity to avoid dependency hell.

{% hint style="info" %}
It's not necessary to strong name your mods, but we recommend you to do so using the strong naming utility (`sn.exe`) that comes with Visual Studio.

Most of the time, you don't have to do anything to your mods, since the signing key change doesn't break any API functions. If you found that your mods no longer worked, try to update to the latest version of Nitrocid, or contact the mod vendor.
{% endhint %}

### Enhanced console wrapper

{% code title="ConsoleWrapperTools.cs" lineNumbers="true" %}
```csharp
public static class ConsoleTools
```
{% endcode %}

When Terminaux shipped its first version, it had a console wrapper that was exclusive to the console reader. We've decided to make this wrapper broader, causing Terminaux to ddepend on this wrapper to call the Console functions, hence increasing its flexibility.

{% hint style="info" %}
`ConsoleTools` has been renamed to `ConsoleWrappers`, and it has been moved to the `Terminaux.Base` namespace.

None of the actions are affected.
{% endhint %}
