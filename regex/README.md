# Regex

* **Backreferences** match the same text as previously matched by a capturing group
	* There is a difference between a backreference to a capturing group that **matched** *nothing*, and one to a capturing group that did not participate in the match at all.
		* `(q?)b\1` matches "b". `q?` is optional and matches nothing, causing `(q?)` to match and capture nothing - so `b` matched "b" and `\1` **matched** nothing (emphasizing the word **matched**.)
		* `(q)?b\1` fails to match "b", because `(q)` fails to match at all so the group never gets to capture anything at all. The engine proceeds to match `b` but eventually fails `\1` because `\1` is not optional.
* **Forward references** allow you to use backreferences to a group that appear later in the regex (there can be situations in which the regex engine evaluates the backreference after the group has already matched.) 
	* `(\2two|(one))+` matches "oneonetwo", `\2` fails in the first group, the second group `(one)` matches. Now the first group is repeated and `\2two` matches, in the end matching "oneonetwo".
* **Alternation** match a single regular expression out of *several possible* regular expressions

#### Branch Reset Groups
> 
> `(?|regex)`
> * `(?|` opens the group
> * `regex` is any regular expression
> `(?|(a)|(b)|(c))` matches `a`, `b`, or `c` (three *alternatives*) with only a **single** capture group (`$1`)
> `(a)|(b)|(c)` matches `a`, `b`, or `c` but there are *three* capture groups
> `(?|(a)|(b)|(c))\1` matches `aa`, `bb`, or `cc`
> `(?|abc|(d)(e)(f)|g(h)i)` has three capture groups
> * `abc` all three capture groups are empty
> * `def` **$1** holds `a`, **$2** holds `b`, **$3** holds `c`
> * `ghi` **$1** holds `h`,  the other groups are empty
> Groups in the branch reset group are numbered continued from the groups before the branch reset group.
> Groups after the branch reset group are numbered continued from the alternative with the most groups.



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

