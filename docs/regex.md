Cheat sheet by:\
Gustav Janér

# Regular Expressions
A regular expression is a sequence of characters that forms a search pattern

##  Mark character
`.`      - Any Character Except New Line

`\d`      - Digit (0-9)

`\D`      - Not a Digit (0-9)

`\w`      - Word Character (a-z, A-Z, 0-9, _)

`\W`      - Not a Word Character

`\s`      - Whitespace (space, tab, newline)

`\S`      - Not Whitespace (space, tab, newline)

##  Mark position
`\b`      - Word Boundary (a space or new line)

`\B`      - Not a Word Boundary

`^`      - Beginning of a String (of a line)

`$`      - End of a String       (of a line)

## Character sets
`[]`    - Matches Characters in brackets (Character Set)

`[^ ]`    - Matches Characters NOT in brackets

`|`    - Either Or

`( )`    - Group (of words)

## Quantifiers
`*`       - 0 or More

`+`       - 1 or More

`?`       - 0 or One

`{3}`     - Exact Number

`{3,4}`   - Range of Numbers (Minimum, Maximum)
