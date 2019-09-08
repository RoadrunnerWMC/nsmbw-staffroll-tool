NSMBW Staffroll Tool
====================

This tool converts New Super Mario Bros. Wii's staffroll.bin to/ from a custom
text file format. It's backward-compatible with text files produced by Treeki's
Staffroll Editor tool, though some new things have also been introduced.

Requires a reasonably recent version of Python 3. (Tested with 3.6.)

staffroll_lib.py can be imported as a standalone Python module, too.


CLI
---

    python3 staffroll.py [-h] [--type {bin,txt}] [--dont-abbr-indents] in_file [out_file]

* -h: display help information
* --type bin, --type txt: the input file type (binary format or text format). If not specified, the tool will make an educated guess based on the file contents.
* --dont-abbr-indents: when converting to text format, the tool will by default omit indentation values when they match the auto-calculated optimal indentation value for centering. If this option is specified, however, indentation levels will always be explicit in the output file.
* in_file: the input file
* out_file: the output file. If not specified, defaults to in_file plus either ".bin" or ".txt" depending on the output format.


Text file format overview
-------------------------

Example:

    :<bold>PLAYTESTING</bold>
    :RoadrunnerWMC
    :Skawo
    :Other People



    :<bold>GENERAL PRODUCER</bold>
    :RoadrunnerWMC



    :<bold>AND OF COURSE</bold>
    :<coin>Original Nintendo Staff</coin>



    :<copyrights>


Blank lines in the file represent blank lines in-game. It's recommended to use
3 blank lines between each block of credits text.

For non-blank lines, the format is

    indent:text

where indent is a non-negative integer that represents the amount of
indentation, measured in blocks from some position on the left (the left edge
of the screen, maybe?). If you leave the value out, the tool will automatically
calculate the correct indentation for centering. (The formula is
`15 - floor(length(text) / 2)`.)

Tags
----

Tags in this format are not parsed as actual XML (though it looks similar). All
are case-insensitive.

### Copyrights tag

    <copyrights>

inserts the copyright text (which is not made of brick blocks). Despite how it
looks, this tag is self-closing; it essentially just represents a special
character in the binary file.

### Bold tag

    <bold>Bold text</bold>

causes the text between the tags to be drawn in a yellow font (used in section
headers). The tag name is `<bold>` only for backward-compatibility; this option
does not actually affect font weight.

### Contents tags

There are three kinds of brick contents tags:

    <coin>These bricks all contain 15 coins</coin>

    <no_coin>These bricks are breakable and empty</no_coin>

    <unbreakable>These bricks cannot be broken at all</unbreakable>

These correspond to values 1, 2 and 3 for the character contents nybble in the
file data. "Unbreakable" seems to be an unused default behavior for any value
greater than 2. Values higher than 3 are not supported by this tool, since they
seem rather useless.

Contents tags cannot be nested within each other (though they can be
nested with `<bold>` tags). Behavior is undefined if you try it.

Value 0 is used for any text not enclosed by contents tags.


License
-------

Licensed under the GNU GPL v3. See the `LICENSE` file for the full license
text.