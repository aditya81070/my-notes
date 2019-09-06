[Home](./index.md) / regular-expression

# Regular Expression Notes

  - [Basics](#basics)
    - [Extracting out the match](#extracting-out-the-match)
    - [Matching with multiple patterns](#matching-with-multiple-patterns)
    - [Matching anything with Wildcard period(.)](#matching-anything-with-wildcard-period)
    - [Match single character with multiple possibilities([])](#match-single-character-with-multiple-possibilities)
    - [Match single characters not specified([^])](#match-single-characters-not-specified)
    - [Match character that occur one or more time(+)](#match-character-that-occur-one-or-more-time)
    - [Match character that occur zero or more time(*)](#match-character-that-occur-zero-or-more-time)
    - [Find Character with lazy Matching(?)](#find-character-with-lazy-matching)
    - [Match Beginning string patterns](#match-beginning-string-patterns)
    - [Match Ending string patterns](#match-ending-string-patterns)
    - [Specify upper and lower limit to match({})](#specify-upper-and-lower-limit-to-match)
    - [Check for All or None(?)](#check-for-all-or-none)
    - [Matching multiple patterns (Lookaheads)](#matching-multiple-patterns-lookaheads)
  - [Shorthand Character classes](#shorthand-character-classes)
  - [Flags](#flags)

## Basics
Regex is always put in `//` and you don't have to put any kind of quotes around the string you want to search. For example, if you want to search `the` in the string `the dog is very good`, you will use `/the/`.

We can use `test` method on a regex and pass it the string we want to check. It will return `true` if pattern is found otherwise `false`.

```javascript
let waldoIsHiding = "Somewhere Waldo is hiding in this text."
let waldoRegex = /Waldo/
let result = waldoRegex.test(waldoIsHiding)
// prints true
console.log(result)
```

### Extracting out the match
You can extract out the substring that matches with regex using `match` function on regex and passing it the string to match.

```javascript
let extractStr = "Extract the word 'coding' from this string.";
let codingRegex = /coding/;
let result = extractStr.match(codingRegex); // returns ["coding"]
// prints coding
console.log(result)
```

### Matching with multiple patterns

You can search for multiple patterns using the `alternation` or `OR` or `|` operator.

```javascript
let petString = "James has a pet cat."
let petRegex = /dog|cat|bird|fish/
let result = petRegex.test(petString)
// prints true
console.log(result)
```

### Matching anything with Wildcard period(.)

The wildcard character `.` will match any one character. It is used when you don't know a specific character at any position.

```javascript
let exampleStr = "Let's have fun with regular expressions!";
let unRegex = /.un/; // this will match fun, gun, sun, mun, tun etc.
let result = unRegex.test(exampleStr);
console.log(result)
```

### Match single character with multiple possibilities([])
`character classes` allows us to define a group of characters we wish to match by placing item inside square `[]` brackets.
If we want to match `bag`, `bug`, `beg` `big` but not `bog`. We will use `/b[aieu]g/`. The `[aieu]` is a character class that will only match the characters `a`, `i`, `e` or `u`.

In a `character class` or `character set` we can use `-` to specify a range of characters like `[a-e]` will match any character between `a` and `e` and including both ends.

```javascript
const regex = /[a-z]/gi // it will match all the letters
```

### Match single characters not specified([^])

We use `negated character set`. To specify this we use `^` or `caret` after the opening bracket and before the character we don't want to match.

```javascript
let quoteSample = "3 blind mice.";
let myRegex = /[^0-9aeiou]/gi; // this will match any character that is not digit and vowel
let result = quoteSample.match(myRegex);
console.log(result)
```

### Match character that occur one or more time(+)

If want to check a character that has appeared one or more time in a row, we can use `+`. For example `/a+/g` will match `a` in `abc`, `aa` in `aabc` and `[a, a]` in `abab` as two `a`'s are not continuos.

### Match character that occur zero or more time(*)
`*` is used same as `+` except one thing that it will match character that appears zero or more time.
For example `/go*/` will match `goooo` in string `gooool` and `g` in string `gut is roof`.

### Find Character with lazy Matching(?)

In regular expression, a `greedy` match finds the longest possible match that fits into the regex pattern and returns a match. The alternative is called `lazy` matching, which finds the smallest possible part of the string that satisfies regex.

We can apply the regex `/t[a-z]*i/` to the string `titanic`. This regex is basically a pattern that starts with t, ends with i, and has some letters in between.

Regular expressions are *by default greedy*, so the match would return `["titani"]`. It finds the largest sub-string possible to fit the pattern.
However, we can use the `?` character to change it to lazy matching.`titanic` matched against the adjusted regex of `/t[a-z]*?i/` returns `["ti"]`.

### Match Beginning string patterns

We can use `^` to specify that pattern should begin matching from starting of the string.
```javascript
let rickyAndCal = "Cal and Ricky both like racing.";
let calRegex = /^Cal/; // Change this line
let result = calRegex.test(rickyAndCal);
console.log(result) // prints true
```

### Match Ending string patterns

We can use `$`  to search the end of string in the regex.

### Specify upper and lower limit to match({})

We can use the `{lower, upper}` to specify the `lower` and `upper` limit to match the characters.
1. **{lower, upper}** - Specify both lower and upper limit.
2. **{lower,}** - Specify only lower limit.
3. **{exact}** - Specify exact number of matches.

### Check for All or None(?)

We can specify the possible existence of an element with question mark `?`. This checks for zero or one of the preceding elements. This is read as that previous symbol is optional.
For example `/colou?r/`. It will match both `color` and `colour`. `/jpe?g` will match both `jpg` and `jpeg`.

### Matching multiple patterns (Lookaheads)

Lookaheads are pattern that tell javascript to look ahead in your string to check for patterns further along. This is useful when we want to search for multiple patterns over the same string.

There are two kinds of lookaheads:-
1. **Positive Lookahead (?=...)** - A `positive lookahead` will look to make sure that the element in search pattern is there. A positive look ahead is used as `(?=...)`where the `...` is the required part that should be there.
2. **Negative Lookahead (?!...)** - A `negative lookahead` will look to make sure that the element in search pattern should not be there. A negative lookahead used as `(?!...)` where the `...` is the pattern that we don't want to there.

Example:-
```javascript
let password = "abc123";
let checkPass = /(?=\w{3,6})(?=\D*\d)/; // looks for between 3-6 character and at least one number
checkPass.test(password); // Returns true
```

## Shorthand Character classes

1. **\w** - `alphanumeric characters match`. This shortcut is equal to `/[a-zA-Z0-9_]/`. This character class matches lowercase, uppercase, digits and `_(underscore)`.
2. **\W** - `non alphanumeric characters match`. This shortcut is equal to `/[^a-zA-Z0-9_]`.
3. **\d** - `digits`. This is equal to `[0-9]`.
4. **\D** - `non-digit characters`. This is equal to `[^0-9]`.
5. **\s** - `matches whitespace`. This will not only match whitespace, but also carriage return, tab, form feed, newline characters. This is similar to `[ \r\t\n\f\v]`.
6. **\S** - `non-whitespace characters`.

## Flags

1. **Ignore case**: The `i` flag. It will ignore the case and will match the string. It is appended to the regex like this- `/ignorecase/i`. This regex can match `ignorecase`, `IgnoreCase`, `igNoreCase` etc.
2. **Global find**: By default `str.match(regex)` will find only first match and return it. If we want to find all matched then we use `g` flag. It is written as `/regex/g`.