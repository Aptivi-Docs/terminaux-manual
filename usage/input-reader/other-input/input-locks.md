---
description: Locking the input...
icon: lock
---

# Input Locks

Terminaux allows you to set an input lock that makes the input system wait until the condition for the lock evaluates to `true`. When you set up an input lock, you essentially tell Terminaux to wait while a procedure in another thread or an instruction inside the lock action evaluates to `true`, before accepting any further input.

***

## <mark style="color:$primary;">Setting default input lock</mark>

In the `Input` class, you can set one of the following properties:

<table><thead><tr><th width="200">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>DefaultLockCondition</code></td><td>Sets the default lock condition action in which Terminaux must evaluate to true before any further input is accepted.</td></tr></tbody></table>

In supported input functions, such as `ReadPointerOrKey()`, `ReadPointerOrKeyUntil()`, `ReadKey()`, and `ReadKeyTimeout()`, you can set the following parameters to change the default behavior:

<table><thead><tr><th width="144.66668701171875">Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>lockCondition</code></td><td>Sets the default lock condition action in which Terminaux must evaluate to true before any further input is accepted.</td></tr><tr><td><code>disableLock</code></td><td>Specifies whether to force accept further input even when Terminaux evaluates the lock condition to <code>false</code>.</td></tr></tbody></table>

{% hint style="danger" %}
Make sure that your lock condition evaluates to `true` at some point, as locks that evaluate to `false` indefinitely can cause an infinite loop when trying to process the next input.
{% endhint %}

***

## <mark style="color:$primary;">Usage with input reader</mark>

You can use input locks with the input reader using the reader settings that derive from the default input settings. You can consult the below page for more info.

{% content-ref url="../reader-settings.md" %}
[reader-settings.md](../reader-settings.md)
{% endcontent-ref %}
