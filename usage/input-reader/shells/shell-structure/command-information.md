---
description: Define this command for me!
icon: info
---

# Command Information

The shell provides a set of overridable properties in your command class that stores information about a specific command that you've created to make your own rules.

***

## <mark style="color:$primary;">Implementation of command information</mark>

To implement your command, you must override those properties in your command class that will store general and optional information about your command.

<details>

<summary>Implementing command information</summary>

Each command you define in your shell must override the abstract properties holding details about the specified command. The required and optional properties are:

<table><thead><tr><th width="190.00006103515625">Variable</th><th width="110">Required</th><th>Description</th></tr></thead><tbody><tr><td><code>Command</code></td><td>●</td><td>The command</td></tr><tr><td><code>HelpDefinition</code></td><td>●</td><td>The brief summary of what the command does</td></tr><tr><td><code>CommandArgumentInfo</code></td><td></td><td>Array of argument information about your command</td></tr><tr><td><code>CommandFlags</code></td><td></td><td>All command flags</td></tr></tbody></table>

</details>

<details>

<summary>Implementing <code>CommandArgumentInfo</code></summary>

To implement `CommandArgumentInfo`, call the constructor either with no parameters, which implies that there is no argument required to run this command, or with the following options listed below.

```csharp
public CommandArgumentInfo()
public CommandArgumentInfo(bool AcceptsSet)
public CommandArgumentInfo(bool AcceptsSet, bool infiniteBounds)
public CommandArgumentInfo(CommandArgumentPart[] Arguments)
public CommandArgumentInfo(CommandArgumentPart[] Arguments, bool AcceptsSet)
public CommandArgumentInfo(CommandArgumentPart[] Arguments, bool AcceptsSet, bool infiniteBounds)
public CommandArgumentInfo(SwitchInfo[] Switches)
public CommandArgumentInfo(SwitchInfo[] Switches, bool AcceptsSet)
public CommandArgumentInfo(SwitchInfo[] Switches, bool AcceptsSet, bool infiniteBounds)
public CommandArgumentInfo(CommandArgumentPart[] Arguments, SwitchInfo[] Switches)
public CommandArgumentInfo(CommandArgumentPart[] Arguments, SwitchInfo[] Switches, bool AcceptsSet)
public CommandArgumentInfo(CommandArgumentPart[] Arguments, SwitchInfo[] Switches, bool AcceptsSet, bool infiniteBounds)
```

where:

<table><thead><tr><th width="160.3333740234375">Variable</th><th>Description</th></tr></thead><tbody><tr><td><code>Arguments</code></td><td>Defines the command arguments</td></tr><tr><td><code>Switches</code></td><td>Defines the command switches</td></tr><tr><td><code>AcceptsSet</code></td><td>Whether to accept the <code>-set</code> switch</td></tr><tr><td><code>infiniteBounds</code></td><td>Whether to accept infinite number of arguments or not</td></tr></tbody></table>

</details>

<details>

<summary>Implementing <code>CommandArgumentPart</code></summary>

For `CommandArgumentPart` instances, consult the below constructor to create an array of `CommandArgumentPart` instances when defining your commands:

```csharp
public CommandArgumentPart(bool argumentRequired, string argumentExpression, Func<string[], string[]> autoCompleter = null, bool isNumeric = false, string[] exactWording = null, string argumentDesc = "")
```

where:

<table><thead><tr><th width="189.6666259765625">Variable</th><th>Description</th></tr></thead><tbody><tr><td><code>argumentRequired</code></td><td>Is this argument part required?</td></tr><tr><td><code>argumentExpression</code></td><td>Command argument expression</td></tr><tr><td><code>autoCompleter</code></td><td><p>Auto completion function delegate<br></p><ul><li>The first <code>string[]</code> denotes the list of last passed arguments</li><li>The second <code>string[]</code> (output) denotes the suggestions returned</li></ul></td></tr><tr><td><code>isNumeric</code></td><td>Whether this argument part accepts numeric values only</td></tr><tr><td><code>exactWording</code></td><td>If not empty, the user must write one of the words declared in this variable for this argument to be satisfied</td></tr><tr><td><code>argumentDesc</code></td><td>Argument description that shows up in the help entry</td></tr></tbody></table>

In case you want to expressively specify the options without having to use default values for all parameters to set a certain parameter, you can use the `CommandArgumentPartOptions` overload:

```csharp
public CommandArgumentPart(bool argumentRequired, string argumentExpression, CommandArgumentPartOptions options)
```

where:

<table><thead><tr><th width="191">Variable</th><th>Description</th></tr></thead><tbody><tr><td><code>argumentRequired</code></td><td>Is this argument part required?</td></tr><tr><td><code>argumentExpression</code></td><td>Command argument expression</td></tr><tr><td><code>options</code></td><td>Options of command argument part</td></tr></tbody></table>

</details>

<details>

<summary>Implementing stricter argument checker</summary>

For commands that require more than just simple argument checking as specified in the `CommandArgumentPart` instances, you can use the `ArgChecker` property to set it to a function delegate that checks all the arguments, with the command parameter info as the first argument.

Such functions must return `0` to continue execution. Else, the command execution will not continue and the last error code will be set to what the function returns.

This is an example for the `alarm` command:

<pre class="language-csharp" data-title="CommandInfo for alarm" data-line-numbers><code class="lang-csharp">[
    new CommandArgumentInfo(
    [
        (...)
    ])
    {
<strong>        ArgChecker = (cp) => AlarmCommand.CheckArgument(cp, "start")
</strong>    },
    (...)
]
</code></pre>

{% code title="Alarm command code" lineNumbers="true" %}
```csharp
internal static int CheckArgument(CommandParameters parameters)
{
    (...)
}
```
{% endcode %}

</details>

***

## <mark style="color:$primary;">Auto-completion for commands</mark>

Commands can have auto-completion set up, so that program users can use their TAB key as means to automatically complete the expression for a command, depending on argument positioning.

<details>

<summary>How the shell selects a completer</summary>

The shell, when TAB is pressed, will select one of the following completers:

* If the auto completer is specified, then, regardless of whether the expression represents the selection (expressions containing the slash `/` character) or not, the auto completer specified in the constructor will be called.
* If the auto completer is not specified, then it will go through the following completers:
  * The shell goes through the list of known completion expressions according to the argument expression, which are the following:
    * `cmd`, `command`: List of all available commands
    * `shell`: List of all available shells
    * `$variable`: List of all MESH variables
  * If the expression is not listed in any of the known expressions list, it'll check for the selection indicator characters (the slash `/` key).
    * For example, the `true/false` expression will generate an auto completer that completes the two words: `true` and `false`.
  * In case there is none, the shell will use the default auto completer, which fetches possible files and folders on your current working directory.

</details>

<details>

<summary>Manipulating with completion functions</summary>

The known expressions list can be manipulated, by registering and unregistering a completion expression. You can use one of the following functions found in the `CommandAutoCompletionList` class:

<table><thead><tr><th width="299.666748046875">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>RegisterCompletionFunction()</code></td><td>Registers the completion function using a name and a function that returns a list of possible completions.</td></tr><tr><td><code>UnregisterCompletionFunction()</code></td><td>Unregisters the completion function by name</td></tr><tr><td><code>IsCompletionFunctionRegistered()</code></td><td>Determines whether the completion function is registered or not</td></tr><tr><td><code>IsCompletionFunctionBuiltin()</code></td><td>Determines whether the completion function is registered as a built-in completer or not</td></tr></tbody></table>

</details>

<details>

<summary>Example of completion implementation</summary>

Here's a simple example as to how to define such completion function:

<pre class="language-csharp"><code class="lang-csharp"><strong>// Completion function registration
</strong><strong>CommandAutoCompletionList.RegisterCompletionFunction("text", (_) => { return ["Hello", "Hi"]; });
</strong>
ShellManager.RegisterShell("TestShell", new TestShellInfo());
ShellManager.StartShell("TestShell");
ShellManager.UnregisterShell("TestShell");

<strong>// Completion function unregistration
</strong><strong>CommandAutoCompletionList.UnregisterCompletionFunction("text");
</strong></code></pre>

</details>
