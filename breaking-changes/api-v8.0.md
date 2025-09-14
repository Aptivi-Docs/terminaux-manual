---
description: Breaking changes for API v8.0
icon: up
---

# API v8.0

Here is a list of breaking changes that happened during the API v8.0 period when differing versions of Terminaux introduced breaking changes.

## From 7.0.x to 8.0.x

Between the 7.0.x and 8.0.x version range, we've made the following breaking changes:

### BassBoom library path settings moved

{% code title="TermReaderSettings.cs" lineNumbers="true" %}
```csharp
public string BassBoomLibraryPath
{
    get => bassBoomLibraryRoot ?? "";
    set => bassBoomLibraryRoot = value;
}
```
{% endcode %}

The BassBoom library path is supposed to be set once, because BassBoom loads the library once before being functional. To reflect BassBoom's behavior, we've decided to move the above property to the static class of `Input`.

{% hint style="info" %}
Update all references to this property to use the new location.
{% endhint %}
