**These notes were taken while or after taking the _Learning Regular Expressions_ course found on [lynda.com](https://www.lynda.com/Regular-Expressions-tutorials/) and taught by [Kevin Skoglund](https://github.com/kevinskoglund).**

----

## Regular Expressions

- Regular Expressions is a formal language interpreted by a regular expression process.
  - It is used for matching, searching, and replacing text.
- Regex is case sensitive.
- It is not a programming language. It is a text manipulation tool, which helps return text only. Therefore, working with numbers can be tricky. See the `[50-99]` example below.

----

### Metacharacters

Metacharacter | What does it do? | Example
------------- | ---------------- | -------
. | Matches any one character, except newline. | `/h.t/` matches "hat", "hot", and "hit". But it does not match "heat".
\ | Escapes the next character. Only for metacharacters Allows use of metacharacters as literal characters. | `/9\.00/` matches "9.00", but not "9500" or "9-00".
[] | Creates a character set which will match any **one** of the characters within the set. | `gr[ea]y` will match `gray` and `grey`, but **will not** match `great`, because only one of the characters within the set will be matched at a time.
`-`| Matches all characters that are between two characters. Only works while within a set `[]`. Outside of a set, it is only a literal dash. | `[0-9]` matches 0 through 9. `[A-Za-z]` matches the alphabet in lower and upper case. However, `[50-99]` does not match the number from 50-99. Regex looks at text only. This will, therefore, only match the numbers between 0 and 9.


----

### Special Characters

- Space: a space is an actual character in regex. So `cat` does not match `c a t`.
- Tabs `\t`: To find tab spaces. To match `a  b`, I'd need to search for `a\tb`.
- Line returns and new lines: If I need to find a line return (when somebody hits "Enter" to start a new line) or a new line, there are a few options:
  - `\r`: Finds a return.
  - `\n`: Finds a new line.
  - `\r\n`: Finds a return and a new line.
