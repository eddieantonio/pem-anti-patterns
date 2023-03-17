---
title: Programming Error Message Anti-patterns
author: Eddie Antonio Santos
---

<style>
/* Hack to make error messages wrap if they're too long. */
pre,pre>code{white-space: pre-wrap !important}
</style>

# Quick links

 - [Abstract](https://dl.acm.org/doi/10.1145/3545947.3576243)
 - [Supplemental video](https://dl.acm.org/doi/10.1145/3545947.3576243#sec-supp)

 - [Compiler-speak](#compiler-speak)
 - [Charged Vocabulary](#charged-vocabulary)
 - [Implicit Suggestion](#implicit-suggestion)
 - [Token Soup](#token-soup)

# Calogue of Anti-patterns

## Compiler-speak

Compiler-speak is when an error message contains **vocabulary that is only
relevant to the implementation of the programming language**. These are
terms you would not hear in an introductory programming classroom,
but more likely to hear in a compiler-construction classroom. Examples
include:

 - token
 - identifier
 - symbol
 - production

There are also terms that reveal details about how the programming
language is actually implemented, rather than concepts necessary to
understand how to program in the language itself. Examples include:

 - bytecode
 - vtable
 - constant pool
 - Any constant internal to the compiler, e.g. `T_RBRACE`,
   `T_MONKEYS_AT`, `T_PAAMAYIM_NEKUDOTAYIM`

### Example of Compiler-speak in PHP 7

```
PHP Parse error:  syntax error, unexpected 'const' (T_CONST), expecting :: (T_PAAMAYIM_NEKUDOTAYIM) in /private/tmp/example.php on line 2
```

`T_CONST` and `T_PAAMAYIM_NEKUDOTAYIM` are the PHP compiler's internal names `const` and  for `::` (the double colon operator), respectively.


### Example of Compiler-speak in C++ (clang)

It describes undefined "symbols" and is complaining of a missing "vtable", both of which are internal details on how C++ is implemented on the particular system this message came from.

<pre>Undefined symbols for architecture arm64:
  &quot;vtable for Duck&quot;, referenced from:
      Duck::Duck() in animals-33cacd.o
  NOTE: a missing vtable usually means the first non-inline virtual member function has no definition.
ld: symbol(s) not found for architecture arm64
clang: <span style="font-weight:bold;color:red;">error: </span><span style="font-weight:bold;">linker command failed with exit code 1 (use -v to see invocation)</span>
</pre>

## Charged Vocabulary

Charged Vocabulary is the use of **unnecessarily harsh, charged, or
graphic word choices** in error messages. The presence of these words are
considered an anti-pattern because they may lead
programmers—especially novices—to believe that the error made is more
severe in nature than it actually is. For example, stating that their
code is “illegal” (breaking the law) rather than just invalid.

Examples include:

- Illegal
- Fatal
- Killed
- Dead
- Abort
- Forbidden
- Terminated

### Example of Charged Vocabulary in JavaScript (SpiderMonkey)

```
Uncaught SyntaxError: illegal character U+201C
```

This character is not “illegal”, that is, it does not break the law.

## Implicit Suggestion

When the wording of an error message can be **misinterpreted as
a suggestion of how to fix the true error, when no such suggestion is
actually intended**. For example, the word "expected" has two senses in
English: "anticipated" or "requested". When an error message says
"Expected _X_", it is intended in the sense of "anticipated _X_" rather than
"requested _X_". However, a beginner may not know this and may take the
"advice". In certain cases, inserting _X_ will introduce even more
problems. Implicit Suggestions are typically phrased:

 - expected _X_
 - _X_ expected
 - missing _X_

An Implicit Suggestion might be misconstrued as a way to solve an error,
but can often causes further errors if its "advice" is taken.

### Example of an Implicit Suggestion in Typescript

Should I insert a variable where the underline is?

<pre><span style="filter: contrast(70%);color:teal;">example.ts</span>:<span style="filter: contrast(70%);color:olive;">2</span>:<span style="filter: contrast(70%);color:olive;">13</span> - <span style="filter: contrast(70%);color:red;">error</span><span style="filter: contrast(70%);color:dimgray;"> TS1134: </span>Variable declaration expected.

<span style="color:white;background-color:black;">2</span>     const = 0;
<span style="color:white;background-color:black;"> </span> <span style="filter: contrast(70%) brightness(190%);color:red;">            ~</span>
</pre>


## Token Soup

Token Soup is when an error message prints a incoherent list of _tokens_. Tokens
are the indivisible textual elements of the programming language (e.g.,
operators, separators, keywords, literals, etc.). Example of tokens from
the Java programming language:

 - `,`
 - `;`
 - `{`
 - `}`
 - `>>>=`
 - `public`
 - `strictfp`

Token Soup occurs when an error message prints a **list of tokens**, often
a list that looks seemingly random or **incoherent**. Often, these are
tokens that the parser is anticipating at a certain position in source
code in order to match a grammar rule, but could not find.

### Example of Token Soup in Rust

<pre><code><span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:red;">error</span><span style="font-weight:bold;">: expected one of `!`, `(`, `)`, `+`, `,`, `::`, or `&lt;`, found `&gt;`</span>
 <span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:blue;">--&gt; </span>example.rs:1:40
  <span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:blue;">|</span>
<span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:blue;">1</span> <span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:blue;">|</span> fn multiply_pairs(pairs: &amp;Vec&lt;(f64, f64&gt;)&gt;) -&gt; Vec&lt;f64&gt; {
  <span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:blue;">| </span>                                       <span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:red;">^</span> <span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:red;">expected one of 7 possible tokens</span>
</code></pre>


`!`, `(`, `)`, `+`, `,`, `::`, or `<` is the token soup.
