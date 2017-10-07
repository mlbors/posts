# Don't fear the mysterious regex #

_Regular expressions_, or _regex_, are something powerful because they allow us to define a search pattern, but they can also be really frightening. Let's try to understand them a little more.

A _regex_ is nothing more than a sequence of characters that defines a search pattern. This pattern can then be used by string searching algorithms for "_find_" or "_find and replace_" operations on strings. The main problem with _regex_ it's that they can really afraid us because we, human beings, have some trouble to read them. So, _we give regex a bad name_.

If we take a closer look at _regex_ on _PHP_ side, we notice that we can use regex of two kinds:

* POSIX Regular Expressions
* PERL Style Regular Expressions (PCRE)

As of _PHP 5.3.0_, the _POSIX Regex extension_ is deprecated, so we are going to make our examples with _PCRE_. _Perl-style_ regular expressions are similar to their _POSIX_ counterparts.

## Hello regex ##

So, let's try to use regular expressions by starting with a really basic example:

    preg_match("/hello world/", "hello world");

This line will return _true_ because here, we are searching the pattern "_hello world_" (first argument) in the string "_hello world_" (second argument). Now, if we use the string "_Hello world_", it will return _false_ because, by default, regex are case-sensitive. We can make them case-insensitive like so:

    preg_match("/hello world/i", "Hello world");

## OR operator ##

Let's go a little further by using the _OR operator_, represented by |.

    preg_match("/hello world|john/i", "Hello world");
    preg_match("/hello world|john/i", "hello John");

Here, both expressions will return _true_.

## Delimiters ##

Now, we can be more precise by using _delimiters_ that will let us define if we want to look at the start of the end of the line.

* ^ targets the line's start
* $ targets the line's end


    preg_match("/^hello world|john/i", "Hello world! It's a sunny day.");
    preg_match("/fun$/i", "regex are fun");

The first expression says that the line must _start_ with "_hello world OR john_" and the second that the line must _end_ with "_fun_".

## Brackets ##

We can now be better wizards if we use _brackets_. _Brackets_ are used to find a range of characters.

    preg_match("/^[a-z]/i", "This line starts with some character.");
    preg_match("/[^0-9]/", "This string doesn't contain any number.");

Both expressions will return _true_. In the second expression, we use the "^" symbol inside brackets. Here, this last one means that we don't want something in our string.

## Quantifiers ##

We are now able to do pretty amazing things, but we can do more with _quantifiers_. _Quantifiers_ can manage the position or the frequency of character sequences or single characters. A _quantifier_ is applied directly to the character that precedes it.

    preg_match("/^hey+/i", "hey");
    preg_match("/^heyy+/i", "hey");
    preg_match("/[0-9]{1,6}/", "123456");
    preg_match("/[0-9]{1,6}/", "1234");
    preg_match("/^a*/i", "bbbb");
    preg_match("/u?/i", "ooo");

If we take a moment, we can notice that quantifiers used some characters like "# ! ^ $ ( ) [ ] { } ? + * . \". So, how can we do if we want to target the "?" character and not use the quantifier. We simply have to escape it!

    preg_match("/how are you\?/i", "How are you?");
    preg_match("/I\'m fine\! And you\?/i", "I'm fine! And you?");

## Conclusion ##

Through some basic examples, we saw how _regular expressions_ work and that, behind their monstrous appearance, they a really powerful. We don't have to be afraid of them anymore.

## Quick reference ##

Down below is a quick reference:

    [abc]       A single character: a, b or c
    [^abc]      Any single character but a, b, or c
    [a-z]       Any single character in the range a-z
    [a-zA-Z]    Any single character in the range a-z or A-Z
    ^           Start of line
    $           End of line
    \A          Start of string
    \z          End of string
    .           Any single character
    \s          Any whitespace character
    \S          Any non-whitespace character
    \d          Any digit
    \D          Any non-digit
    \w          Any word character (letter, number, underscore)
    \W          Any non-word character
    \b          Any word boundary character
    (...)       Capture everything enclosed
    (a|b)       a or b
    a?          Zero or one of a
    a*          Zero or more of a
    a+          One or more of a
    a{3}        Exactly 3 of a
    a{3,}       3 or more of a
    a{3,6}      Between 3 and 6 of a

    options:
    i           case insensitive 
    m           make dot match newlines 
    x           ignore whitespace in regex 
    o           perform #{...} substitutions only once

## Some tools ##

The following websites can help to build regex:

* [PHP Live Regex](http://www.phpliveregex.com/)
* [Regex 101](https://regex101.com/)
* [RegExr](http://regexr.com/)