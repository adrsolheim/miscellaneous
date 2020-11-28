### Troubleshooting weird behavior
* break the regular expression down into it's individual components.
* speak out loud the steps of the expression.
* build the regular expression incrementally, testing as you go.
* create the correct test data (contains edge cases and not only easy matches)
* most systems use a character table like abcdef..xyzABCD..XYZ, but some has a character table like aAbBcC..xXyYzZ, check this if bugs.
* look for character you forgot to escape if you get unexpected behaviour.
* regular expressions don't consider the meaning of the content, only if a string of characters match the given pattern.
* \n and \r are present in some regular expression implementation but not in others. If you are getting some weird behaviour it could be that they are not implemented in the particular tool you are using.
* inside the character class `[ ]` any character except `^-]\` is treated as a regular character to match for.

**metacharacters** - characters which have a special meaning.
* `.`       matches any single character (l..e -> live, like)
* `[]`      range of characters where any single character matches: [1-4] 1,2,3,4 [1-39] 1,2,3,9 [a-cX] a,b,c,X
* `[^]`     range of characters NOT matching [^0-7] 8,9
* `*`       item occurs zero or more times
* `+`       item occurs one or more times
* `?`       item occurs zero or one time
* `{5}`     item occurs five times
* `{3,7}`   item occurs between 3 and 7 times
* `{2,}`    item occurs at least 2 times
* `\`       remove special meaning of metacharacter, or give special meaning to character

