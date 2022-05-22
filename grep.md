#grep is one of the most useful and powerful commands in Linux for text processing. grep
searches one or more input files for lines that match a regular expression and writes each matching line to standard output.

## Grep Regular Expression
A regular expression or regex is a pattern that matches a set of strings. A pattern consists of operators, constructs literal characters, and meta-characters, which have special meaning. GNU #grep supports three regular expression syntaxes, Basic, Extended, and Perl-compatible.

In its simplest form, when no regular expression type is given, #grep interpret search patterns as basic regular expressions.To interpret the pattern as an extended regular expression, use the -E (or --extended-regexp) option.

In GNU's implementation of #grep there is no functional difference between the basic and extended regular expression syntaxes. The only difference is that in basic regular expressions the meta-characters ?, +, {, |, ( , and ) are interpreted as literal characters. To keep the meta-characters' special meanings when using basic regular expressions, the characters must be escaped with a backslash (\). 

```
$ grep bash /etc/passwd 
```

```
Output
root:x:0:0:root:/root:/bin/bash
dhiks:x:1000:1000:dhiks,,,:/home/dhiks:/bin/bash
```
