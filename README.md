BPM Library: ReadKey
====================

Read keys and escape sequences (represented as codes) from the keyboard.


Installation
============

Add to your `bpm.ini` file the following dependency.

    [dependencies]
    readkey=*

Run `bpm install` to add the library. Finally, use it in your scripts.

    #!/usr/bin/env bash
    . bpm
    bpm::include readkey

Alternately, you may download `libreadkey` and source it into your environment (or include it however you want in your shell scripts). The use of [BPM] is not necessary.

A special note is that this library does not use the [`assign`] library. Because of this design choice, you should not use variables that start with `__readkey__*` in code that uses this library. It was done in order to boost the speed. When reading characters from the terminal, having a really fast response time is important, especially when obtaining escape sequences. This is not a preferred practice; using [`assign`] is far safer.


API
===


`readkey::char()`
-----------------

Reads a single character from the keyboard.

* $1 - Variable name that will be assigned a value when a character is read.

Returns success if a character was read.


`readkey::fast()`
-----------------

Reads a single character from the keyboard, but with a short timeout. Used to get subsequent characters in an escape sequence.

* $1 - Variable name that will be assigned a value when a character is read.

Returns success if a character was read.


`readkey::sequence()`
---------------------

Reads a single character sequence from the keyboard. When an escape sequence is encountered, the follow-up characters are also read.

* $1 - Variable name that will be assigned the sequence.


`readkey::code()`
-----------------

Reads a single character sequence from the keyboard. When a known sequence is encountered, a token is returned instead. Tokens are returned for simple things like `ENTER`, `TAB`, and `ESCAPE`. When arrow keys, function keys, and other special keys are used, they send escape sequences and those are also returned using tokens like `ARROW_LEFT`, `KEYPAD_F1` and `PAGE_UP`. Sometimes modifiers can also be detected, as is the case with `F14+CONTROL` and `BACKSPACE+SHIFT`.

* $1 - Variable name that will be assigned a single character or a token.
* $2 - If set to a non-empty value, the complex codes will be returned. Otherwise, the codes are simplified.

A simpler version will remove the modifier keys from the right, any `KEYPAD_` notation from the left and even map some codes to typical characters.

| Complex Code           | Simple Code |
|------------------------|-------------|
| ARROW_UP+SHIFT         | ARROW_UP    |
| F1+APPLICATION         | F1          |
| KEYPAD_ENTER           | ENTER       |
| KEYPAD_ADD+APPLICATION | +           |

Examples

    # Read a single character
    if readkey::code typed; then
        # Something was typed.
        # $typed can be a token or a single letter.
        echo "You typed: $typed"
    fi

    # Read a complex code
    if readkey::code typed x; then
        echo "You typed: $typed"
    fi


License
=======

This project is placed under an [MIT License](LICENSE.md).

[`assign`]: https://github.com/bpm-rocks/assign
[BPM]: https://github.com/bpm-rocks/bpm
