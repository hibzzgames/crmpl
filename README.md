# papr
Papr is a file format designed to store readable key-value pairs that can be minified.

### Goals
- Simple rules
- Readable
- Can be minified

<br />

P.S. papr stands for Primitive Array of Pair Records!

---

### Rules
**Rule 1:** The colon symbol (`:`) is used to increase the depth of the next token

**Rule 2:** The comma symbol (`,`) is used to keep the depth of the next token at the same level

**Rule 3:** The semicolon symbol (`;`) is used to decrease the depth of the next token

**Rule 4:** Tokens created using the double-quotes symbol (`"`) are terminated by another double-quotes symbol. A double quote can be included inside this wrapped token using a backslash-assisted escape sequence (`\"`). Similarly, a backslash can be included inside this wrapped token using two backslashes (`\\`).

<br />

### Detailed Explanation
With papr, the key-value relationships between the various tokens in a file are defined using a concept called depth. The colon symbol, `:`, is used to increase the depth of the next token and create a child-parent relationship between the current token and the previous token.

```
year: 2024
```

Here, the `:` symbol after the "year" token increases the depth of the next token, "2024", creating a new child-parent relationship where the token "year" is the key and the token "2024" is the value.

The comma symbol, `,`, is used to keep a series of tokens in the same depth. These are used to create arrays of values associated with a particular key.

```
seasons: spring,
         summer,
         fall,
         winter
```

After creating a key-value relationship between the tokens "seasons" and "spring" using the `:` symbol, the `,` symbol after the token "spring" is used to keep the next token "summer" in the same depth as the previous token. The series of `,` symbols are used to chain together and keep a set of tokens in the same depth.

Finally, the `;` token is used to decrease the depth of the next token. This helps create a new key or create values associated with a key in a lower depth.

```
year: 2024;
month: March
```

Here, the `;` symbol after the token "2024", ensures that the next token, "month", moves down a level. This ensures that the tokens "year" and "month" are on the same level and have a sibling-type relationship. The `:` symbol after the token "month" makes it the parent of a new key-value pair relationship.

One last rule is required to clarify what determines a token. To facilitate the existence of the symbols `:`, `,`, and `;` in a token, a complex token can be created by using double quotes (`"`) symbols to wrap content together. Inside a double-quote wrapped complex token, a new double quote can be included by creating an escape sequence with a backslash - `\"`. Subsequently, to include a backslash in a complex token, two consecutive backslashes (`\\`) must be used.

Here's a final sample of a somewhat complicated papr file.

```
AppName: Some App;
Authors: Jane,
         John;
Buttons: 0: id: new;
            fn: newDoc();
            icon: plus;;
         1: id: missing;
            icon: alarm;;
         2: id: edit;
            fn: editDoc();
            icon: pencil;;;
Description: "This is a random description for \"Some App\".";
Version: 1.3.7;
```

## Available Parsers
This repository also contains parsers to encode and decode .papr files for popular languages. The following languages are available:

- none

#### Planned Languages:
- c/c++
- c#
- rust
- js