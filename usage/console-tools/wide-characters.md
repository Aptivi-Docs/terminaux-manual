---
description: Characters that take up more than 16 bits!
icon: square-t
---

# Wide Characters

Wide characters are UTF-32 characters that are usually formed by surrogate pairs, which are connected with each other to form a single character not normally available in the UTF-16 encoding, such as what the predefined 2-byte `char` type in .NET is using. Wide characters take up 4 bytes, which means 32 bits. They are used in simple emojis and other characters, and Terminaux provides a solution to handle these characters, which is a `WideChar` struct. It's found in the `Terminaux.Base.Structures` namespace.

{% hint style="warning" %}
While `WideChar` provides 32-bit characters, some of them, such as color-toned emojis or modified emojis, come with multiple characters, such as zero-width joiners, that can't be represented in a single `WideChar` struct. Terminal emulators provide little to no support for such cases, and Terminaux won't support such characters, so they show up as multiple characters in the terminal.
{% endhint %}

`WideChar` internally uses two 16-bit characters to indicate this wide character in its separated form:

* Low character: First two bytes of the whole wide character (Bytes 1 and 2), and is always populated unless the source character is a NUL character.
* High character: Last two bytes of the whole wide character (Bytes 3 and 4), and is always NUL unless the source exceeds the UTF-16 maximum size.

{% hint style="info" %}
According to Terminaux, high surrogate characters are "low characters" (due to no usage in normal ASCII characters) and low surrogate characters are "high characters" (due to frequent usage), because a `WideChar` instance doesn't necessarily represent a surrogate character, such as normal ASCII characters. However, you can't get these individual high or low characters directly as they are used internally for operations, but you can use implicit casting to a tuple of two characters.
{% endhint %}

You can create an instance of a wide character from one of the following sources:

* Source string representing a single wide character or a single normal character.
* Character code representing a result of logical OR of the high character integral value with 16 bits shifted to the left and the low character integral value without any bit shift: $$(Hi << 16) | Lo$$
* Two separate characters indicating a high character, which comes first in the parameters, and a low character.

You can either create a new instance manually using the `new` keyword or using explicit casting from the following types:

* String instances that represent a single wide character or a single normal character.
* Character code as mentioned above.

In addition to that, you can perform implicit casting to the following types:

* A string to get the resultant character formed by putting the low and the high characters together, or just the low character if it's not a wide character.
* A tuple of characters to get two characters with the high character first and the low character second.
* An integer to get the character code as mentioned above.

You can perform the following operators:

* `==`: Tests for equality of the two `WideChar` instances
* `!=`: Tests for inequality of the two `WideChar` instances
* `<`: Checks to see if the first `WideChar` is less than the second `WideChar` by comparing their character codes
* `<=`: Checks to see if the first `WideChar` is less than or equal to the second `WideChar` by comparing their character codes
* `>`: Checks to see if the first `WideChar` is greater than the second `WideChar` by comparing their character codes
* `>=`: Checks to see if the first `WideChar` is greater than or equal to the second `WideChar` by comparing their character codes
* `+`: Performs the addition of the two `WideChar` instances
* `-`: Performs the subtraction of the two `WideChar` instances

You can also perform the following checks:

* `IsValidChar()`: Checks to see if this character is a valid character
* `IsValidSurrogate()`: Checks to see if this character is a valid surrogate pair of both the low character (high surrogate) and the high character (low surrogate)

{% hint style="info" %}
You can perform the usual `Parse()` and `TryParse()` function calls against either a string, a character code, or high and low characters, with the high character being the first and the low character being the second.
{% endhint %}
