# crmpl
Crmpl is an open file format designed to store readable key-value pairs that can be minified.

### Goals
- Simple rules
- Readable
- Can be minified

<br />

P.S. Crmpl started as an early candidate for [papr](https://github.com/hibzzgames/papr), but the two projects diverged as papr compromised minifiability for readability. Crmpl attempts to strike a balance and that's why crmpl is named as such, like the crumpling of a paper.

---

### Rules
**Rule 1:** The colon symbol (`:`) increases the depth of the next token

**Rule 2:** The comma symbol (`,`) keeps the depth of the next token at the same level

**Rule 3:** The semicolon symbol (`;`) decrease the depth of the next token

**Rule 4:** A token is a series of characters ending with `:`, `,`, `;`, or `#`, with no leading or trailing spaces. A token can include special characters and additional spaces if enclosed in double quotes. The `\"` sequence represents a double-quote within it.

**Rule 5:** A single-line comment starts with a `#`, and a multi-line comment starts and ends with `##`.

<br />

### Detailed Explanation
**Rule 1**

With crmpl, the key-value relationships between the various tokens in a file are defined using a concept called depth. The colon symbol, `:`, is used to increase the depth of the next token and create a child-parent relationship between the current token and the previous token.

```
year: 2024
```

Here, the `:` symbol after the "year" token increases the depth of the next token, "2024", creating a new child-parent relationship where the token "year" is the key and the token "2024" is the value.

<br />

**Rule 2**

The comma symbol, `,`, keeps a series of tokens in the same depth. They create arrays of values associated with a particular key.

```
seasons: spring,
 summer,
 fall,
 winter
```

After creating a key-value relationship between the tokens "seasons" and "spring" using the `:` symbol, the `,` symbol after the token "spring" is used to keep the next token, "summer", in the same depth as the previous token. The series of `,` symbols chains together and keeps a set of tokens in the same depth.

<br />

**Rule 3**

Finally, the `;` symbol decreases the depth of the next token. It helps create a new key or values associated with a key in a lower depth.

```
year: 2024;
month: March
```

Here, the `;` symbol after the token "2024" ensures that the next token, "month", moves down a level. This ensures that the tokens "year" and "month" are on the same level and have a sibling-type relationship. The `:` symbol after the token "month" makes it the parent of a new key-value pair relationship.

<br />

**Rule 4**

Now, these 3 basic rules require some clarity on what is considered a token. By default, a token is a string of characters that are terminated by one of the reserved symbols (`:`, `,`, `;`, or `#`). To make the file cleaner and more readable, the leading and trailing white spaces of a character sequence string will be trimmed. Spaces in the middle will be considered to be part of the token.

```
 licenses:  MIT,  Creative  Commons, Custom   ;
___         __    __                 _ _      ___    <-- trimmed spaces
 --------X  ---X  ----------------- X ------   X   <-- Xs are terminators, dashes are part of a token
 |          |     |                   | 4: Custom
 |          |     | 3: Creative  Commons
 |          | 2: MIT
 | 1: licenses
```

Additionally, to facilitate the existence of reserved symbols or leading/trailing spaces in a token, a complex token can be created using a set of double quotes to wrap the content together. Inside a double-quote wrapped complex token, a new double-quote can be included by creating an escape sequence with a backslash (`\"`).

```
statement:  " This is a complex token with a reserved symbol like a semicolon, \";\", and leading and trailing spaces  "  ;
 __                                                                                                            __     <-- trimmed spaces
---------x  x----------------------------------------------------------------------------------------------------------x  x    <-- Xs are terminators, dashes are part of a token
|            | 2:  This is a complex token with a reserved symbol like a semicolon, ";", and leading and trailing spaces       <-- has 1 leading and 2 trailing spaces
| 1: statement
```

<br />

**Rule 5**

Lastly, we need a way to define comments. Comments are useful in giving context to readers and can be ignored by a parser reading a file. A single-line comment is created using the hashtag symbol (`#`), and by the nature of a single-line comment, it is terminated by a new line.

```
# build info
platform: windows;
versions: 10, 11;
compiler: msvc; # explore replacing with g++ 
```

A single-line comment cannot exist in a minified crmpl file. So, crmpl introduces the double hashtag sequence (`##`) to define the start and the end of a multiline comment. Honestly, these comments can be removed from a minified file, but just in case, from a usability perspective, we want to support it for the users.

```
format: crmpl;
parser: c++ ## WIP ##, c#, js, rust;
```

<br />

These are all the rules required to create and define data in your very own crmpl file. Here's a final sample of a somewhat complicated crmpl file.

```
# an example .crmpl file
AppName: Some App;
Authors: Jane,
 John;
Buttons: 0: id: new;
 fn: newDoc();
 icon: plus;;
 1: id: missing;
 icon: alarm;; ## issue: fix this ##
 2: id: edit;
 fn: editDoc();
 icon: pencil;;;
Description: "This is a random description for \"Some App\".";
Version: 1.3.7;
```
