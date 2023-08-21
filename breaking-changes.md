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
