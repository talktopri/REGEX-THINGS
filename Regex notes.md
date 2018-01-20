**These notes were taken while or after taking the _Learning Regular Expressions_ course found on [lynda.com](https://www.lynda.com/Regular-Expressions-tutorials/) and taught by [Kevin Skoglund](https://github.com/kevinskoglund).**

----

## Regular Expressions

- Regular Expressions is a formal language interpreted by a regular expression process.
  - It is used for matching, searching, and replacing text.
- Regex is case sensitive.
- Regular Expressions are eager and greedy. See respective sections below.
- It is not a programming language. It is a text manipulation tool, which helps return text only. Therefore, working with numbers can be tricky. See the `[50-99]` example below.
- Metacharacters inside character sets are usually treated as literals (already escaped). There is no need to escape them again. They no longer have their metacharacter meaning. Ex: `/h[abc.xyz]t/` matches "hat" and "h.t", but does not match "hot" because the `.` within the set is not a wildcard.
  - Exceptions: `]`, `-`, `^`, and `\`. These characters need to be escaped within a character set

----

## Metacharacters

Metacharacter | What does it do? | Example
------------- | ---------------- | -------
`.` | Matches any one character, except newline. | `/h.t/` matches "hat", "hot", and "hit". But it does not match "heat".
`\` | Escapes the next character. Only for metacharacters Allows use of metacharacters as literal characters. | `/9\.00/` matches "9.00", but not "9500" or "9-00".
`[]` | Creates a character set which will match any **one** of the characters within the set. | `/gr[ea]y/` will match `gray` and `grey`, but **will not** match `great`, because only one of the characters within the set will be matched at a time.
`-`| Matches all characters that are between two characters. Only works while within a set `[]`. Outside of a set, it is only a literal dash. | `/[0-9]/` matches 0 through 9. `/[A-Za-z]/` matches the alphabet in lower and upper case. However, `/[50-99]/` does not match the number from 50-99. Regex looks at text only. This will, therefore, only match the numbers between 0 and 9.
`^` | Matches any one character that **is not** in the set. | `/[^A-Z0-9]/` would match anything that **does not** contain upper case letter and **does not** contain numbers. So only text in lower case, spaces and especial characters would be found. Another example: `/see[^mn]/` would match `see `, but it would not match `seen`, `see`(without the space at the end) and `seem`.



### Special Characters

- Space: a space is an actual character in regex. So `/cat/` does not match `c a t`.
- Tabs `\t`: To find tab spaces. To match `a  b`, I'd need to search for `/a\tb/`.
- Line returns and new lines: If I need to find a line return (when somebody hits "Enter" to start a new line) or a new line, there are a few options:
  - `\r`: Finds a return.
  - `\n`: Finds a new line.
  - `\r\n`: Finds a return and a new line.

### Metacharacters inside character sets

Most metacharacters are already escaped when they are inside a character set:
- `/h[abc.xyz]t/` matches `hat` and `h.t`, but does not match `hot`, because the `.` character is **not** a metacharacter. Since the `.` is inside the set, it is automatically escaped (ignored).

**Exceptions:**

These characters need to be escaped manually:
- `]`: Closing square bracket only. (The opening square bracket is escaped automatically when found inside of a set).
- `-`: Dash, or range metacharacter.
- `^`: Caret
- `\`: Back slash. This gets tricky, because this is the metacharacter used to escape other metacharacters including itself. So, in order to escape `\`, I'd need to use a second `\`, like this: `\\` :scream:  

Ex: Write a regular expression to match `file01 file-1 file\1 file_1`

Answer: `/file[0\-\\_]1/`
Explanation by parts:
- `file[0`: creates the match for the first part of our example: `file0`
- `file[0\-`: here the `\` escapes the `-`, to make it a literal. This way, it would  match `file-`.
- `file[0\-\\]`: here the first `\` escapes the second `\` to make it a literal. This way, it would  match `file\`.
- `file[0\-\\_]1`: The full regular expression. Note that `_` isn't a metacharacter and therefore, does not need to be a concern it. The `]` closes the set and `1` will conclude the expression.

----

## Shorthand Character sets

Shorthand | Meaning | Equivalent
------------- | ---------------- | -------
\d | Digit | [0-9]
\w | Word character | [a-zA-Z0-9_]
\s | Whitespace | [ \t\r\n] (space ` `, tab `\t`, return `\r` or new line `\n`)
\D | Not digit | [^0-9]
\W | Not word character | [^a-zA-Z0-9_]
\S | Not whitespace | [^ \t\r\n]

**Notes:**

`\w`
- Underscore (_) is a word character.
- Hyphen (-) is **not** a word character.
- Digits are word characters.

Ex:
- `/\d\d\d\d/ matches "1984", but not "text". Because `\d` looks for digits only.
- `/\w\w\w/` matches "ABC", "123", and "1_A". Because numbers and the underscore character are considered word characters.
- `/\w\s\w\w/` matches "I am", but not "Am I". Because it looks for one word character followed by a space character.
- `/[\w\-]/` matches one word character or a hyphen.
- `/[\d\s]/` matches any one digit or whitespace character.
- `/[^\d]/` is the same as `/\D/` and `/[^0-9]/`.

:exclamation: Attention: `/[^\d\s]/` is **not** the same as `[\D\S]`:
  - `/[^\d\s]/` means not a digit NOR not a whitespace character. The whole set is negated.
  - `[\D\S]` means EITHER not a digit OR NOT a whitespace character.  

### POSIX

These expressions are not as used as REGEX, but could be useful. You never know :shrug:

Class| Meaning | Equivalent
------------- | ---------------- | -------
[:alpha:] | Alphabetic characters | A-Za-z
[:digit:] | Numeric characters | 0-9
[:alnum:] | Alphanumeric characters | A-Za_z0-9
[:lower:] | Lowercase alphabetic characters | a-z
[:upper:] | Uppercase alphabetic characters | A-Z
[:punct:] | Punctuation characters |
[:space:] | Space characters | \s
[:blank:] | Blank characters (space, tab) |
[:print:] | Printable characters, spaces |
[:graph:] | Printable characters, no spaces |
[:cntrl:] | Control characters (non-printable) |
[:xdigit:] | Hexadecimal characters | A-Fa-f0-9

----

## Repetition metacharacters

Metacharacter | Meaning
------------- | -------
`*` | Preceding item appears zero or more times
`+` | Preceding item appears one or more times
`?` | Preceding item appears zero or one times

Examples:

- `/apples*/` matches "apple", "apples" and "applesssss".
- `/apples+/` matches "apples" and "applesssss", but does not match "apple".
- `/apples?/` matches "apple" and "apples", but does not match "applesssss".
- `/\d\d\d*/` matches numbers with 3 digits or more. i.e: 123, 1234, 12345, 123456 ...

### Quantified Repetition

Use curly braces to create a repetition set: {min,max}. A minimum value is **always** required and it can be equal to 0. A maximum value is not required, as it can be infinite.

Examples:

- `\d{4,8}` matches numbers with four to eight digits.
- `\d{4}` matches numbers with four digits only.
- `\d{4,}` matches numbers with four or more digits.
- `\w{5}\s` matches 5 word characters followed by a whitespace character.
- `\d{3}-\d{3}-\d{4}` would match phone numbers like 555-867-5309.

### Greedy Expressions

Match as much as possible before giving control to the next expression part.

Examples:

- `/.+\.jpg/` matches "filename.jpg"
  - The `+` is greedy, but "gives back" the `.jpg` to make the match.
  - Think of it as rewinding or backtracking.
- `/.*[0-9]+/` matches "Page 266".
  - `/.*/` matches "Page 26" while `/[0-9]+/` matches "6".

### Lazy Expressions

Match as little as possible before giving control to the next expression part.

Metacharacter | Meaning
------------- | -------
? | Makes preceding quantifier lazy (could match 0 characters or 1 character)

**Syntax:**

- *?
- +?
- {min,max}?
- ??

Examples:

- `/apples??/`: The first `?` means that this expression will match "apple" or "apples" (0 or 1 character). However, because regular expressions are greedy, one `?` means it prefers to return "apples".
The second `?` will tell the expression to be lazy and return zero `s`, matching, therefore, "apple".
