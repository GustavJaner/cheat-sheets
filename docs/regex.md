_Author: Gustav Janér_

# Regular Expressions
A regular expression is a sequence of characters that forms a search pattern


##  Mark character
`.` &nbsp; - Any Character Except New Line

`\d` - Digit (0-9)

`\D` - Not a Digit (0-9)

`\w` - Word Character (a-z, A-Z, 0-9, \_)

`\W` - Not a Word Character

`\s` - Whitespace (space, tab, newline)

`\S` - Not Whitespace (space, tab, newline)


##  Mark position
`\b` - Word Boundary (a space or new line)

`\B` - Not a Word Boundary

`^` &nbsp; - Beginning of a String (of a line)

`$` &nbsp; - End of a String       (of a line)


## Character sets
`[]` &nbsp; &nbsp; - Matches Characters in brackets (Character Set)

`[^ ]` - Matches Characters **not** in brackets

`|` &nbsp; &nbsp; &nbsp; - Either or

`( )` &nbsp; - Group (of words)


## Quantifiers
`*` &nbsp; &nbsp; &nbsp; &nbsp;- 0 or More

`+` &nbsp; &nbsp; &nbsp; &nbsp;- 1 or More

`?` &nbsp; &nbsp; &nbsp; &nbsp;- 0 or One

`{3}`  &nbsp; &nbsp;- Exact Number

`{3,4}`- Range of Numbers (Minimum, Maximum)
