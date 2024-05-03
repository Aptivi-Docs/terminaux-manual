---
description: Breaking changes for API v4.0
---

# ⬆️ API v4.0

Here is a list of breaking changes that happened during the API v4.0 period when differing versions of Terminaux introduced breaking changes.

## From 3.4.x to 4.0.x

Between the 3.4.x and 4.0.x version range, we've made the following breaking changes:

### Removed enclosed versions of color sequence properties

{% code title="Color.cs" lineNumbers="true" %}
```csharp
public string PlainSequenceEnclosed
public string PlainSequenceEnclosedTrueColor
```
{% endcode %}

The plain sequences can already be enclosed in double quotes by escaping them in between the sequences, so these two properties are no longer considered useful. In earlier versions of Nitrocid, they were implemented as a workaround of handling `.ini` files that were responsible of handling Nitrocid 0.0.4 to 0.0.15 configuration.

{% hint style="info" %}
We advise you to manually enclose the `PlainSequence` and/or the `PlainSequenceTrueColor` properties with double quotes.
{% endhint %}

### Merged various VT sequence tools

{% code title="VtSequenceTools.cs" lineNumbers="true" %}
```csharp
public static string FilterVTSequencesMultiple(string Text, string replace = "", VtSequenceType types = VtSequenceType.All)
public static (VtSequenceType, Match[])[] MatchVTSequencesMultiple(string Text, VtSequenceType type = VtSequenceType.All)
public static bool IsMatchVTSequencesMultiple(string Text, VtSequenceType type = VtSequenceType.All)
```
{% endcode %}

We've made a refactor of various VT sequence tools in order to be more effective, so we've merged the `Single` and the `Multiple` versions together for the above functions in order to make them more efficient. Some of the tools remained unaffected.

{% hint style="info" %}
We advise you to change how you return the values as appropriate. It's almost exactly the same as the `Multiple` versions, but without the `Multiple` prefix.
{% endhint %}

### Removed non-standalone console writer wrappers

{% code title="ConsoleWrapperTools.cs" lineNumbers="true" %}
```csharp
public static Action<string, TermReaderSettings> ActionWriteStringNonStandalone
public static Action<string, TermReaderSettings, object[]> ActionWriteParameterizedNonStandalone
public static Action<string, TermReaderSettings> ActionWriteLineStringNonStandalone
public static Action<string, TermReaderSettings, object[]> ActionWriteLineParameterizedNonStandalone
```
{% endcode %}

We've finally removed the `TermReader`-exclusive console writer console wrappers as they were initially never meant to be overridden. It was due to Terminaux's reliance on internal functions in the non-standalone console writer wrapper. It was converted to a simple private function in the reader class.

{% hint style="danger" %}
We advice you to stop overriding the above four wrappers. If you rely on these wrappers, you should change your application code to make better use of Terminaux's tools to avoid having to override the standard wrappers for the terminal reader.
{% endhint %}
