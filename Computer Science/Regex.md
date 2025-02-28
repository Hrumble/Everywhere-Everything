*Regular Expression*

**I made a regex game, find it on** [sneaky's regex game](regexgame.sneaky-design.com)

Regex is a pattern matching language. It is useful to process and/or parse data, for example:
![[regex_example.png]]
*Cool except it gets really complex really fast.*

```sh
map _U :'a,'bs/^* /*S/^M:'a,'bs/^*S/* /^M:'a,'bs/^*[ ]*//g^M:'a,'bs/[ ]*\*[]*$//g^M
```
*This is an expression that does god knows what i found on Quora, pretty cool right?*

Now let's learn to actually use it.

# Quantifiers
Quantifiers are the parts of the expression that indicate how many times, if any an element can repeat.

- **a|b** - Matches either "a" or "b"
- **?** - zero or one
- **+** - at least one
- **\*** - zero or more
- **{N}** - Exactly N times
- **{N, }** - N or more
- **{N, M}** - between N and M *including N and M*
- **\*?** - Zero or more, but stop after the first match.

One thing to note is that the quantifiers quantify the expression that comes **right before it**, not the entire expression, so for instance:

```regex
Hi+
```

will match 
```
Hi
Hiiiiiiiiiiiiiiiiiii
Hiiiiii
[...]
```
but it won't match
```
HiHi
```
*technically it would match both separately because Hi is still valid in the context of 'Hi+'*


Let's try using them:

```regex
Hello|Goodbye
```
Will match both hello and goodbye

```regex
Fuck You{3,50}
```
Will match any `Fuck Youuu` with at least 3 `u` and max 50 so `Fuck Youuuuuuu`  but not `Fuck You`

```regex
Hate?
```
will match `Hate` and `Hat`

You can also match the expressions together:
```regex
He?llo{2}
```
will match `Hlloo` or `Helloo`

*you get the gist of it*

## Lazy Matching

So far we've been using what's called **Greedy Matching**, which as the name implies means that it will match with as much as it can.

So `Hi+` will match anything from `Hi` to `Hiiiiiiiiiiiiiii`. We can change it's behavior to **Lazy Matching** by adding a `?`
`Hi+?`
This will make it so that regex will stop searching as soon as it find the first possible match.

Now even if you type in `Hiiiiiiiiiiii` it will only match the first `Hi`.
On it's own it's pretty useless, but becomes powerful when we combine it with other more useful symbols like the `.`.

`.` means **any** character. e.g. `H.llo` will match `Hello Hpllo Hsllo [...]` and so on wtv.

writing `.*` means literally everything, but adding `?` `.*?` will stop at the first character since it matches the pattern.

# Pattern Collection

```regex
I love [uioe]
```
would match `I love u I love i I love o I love e` but nothing else.

You can also specify a logical range such as `[A-Z]` *every uppercase character* `[0-9]` *numbers from 0 to 9*
you can put multiple ranges together to match for either of them `[a-z0-9]` any lowercase letter or number.

*By logical range, it means by the order of ascii characters. this means that `[a-z]` actually matches the ascii character code from `a - 97` to `z - 122`*

technically you could write a `[9-a]` but it would match any character between `9 - 57` and `a - 97` so `< - 60` for instance.

You can also use **negative expressions** with the `^` symbol at the beginning. `[^a-z]` this means anything that is not `[a-z]`

# General Tokens

Those are *tokens* used to identify special characters which you can not identify using normal characters.
- `.` – Any character
- `\n` – Newline character
- `\t` – Tab character
- `\s`– Any whitespace character (including `\t`, `\n` and a few others)
- `\S` – Any non-whitespace character
- `\w`– Any word character (Uppercase and lowercase Latin alphabet, numbers 0-9, and `_`)
- `\W`– Any non-word character (the inverse of the `\w` token)
- `\b`– Word boundary: The boundaries between `\w` and `\W`, but matches in-between characters *see below*
- `\B`– Non-word boundary: The inverse of `\b`
- `^` – The start of a line
- `$` – The end of a line 
- `\`– The literal character “\”

so matching any first letter of each world could be 
```regex
\s.
```
*would match a whitespace as well as any character that comes directly after*

**\b** is a bit special, it doesn't actually match anything, but rather marks boundaries, so for instance you could have `\b\w+\b` which would select every *word characters* between two word boundaries. 
**^ and $** work similarly.

# Character Escaping

You're getting used to that by now but this is basically to match characters that are token themselves, so for instance trying to match `\n`, you would have to escape it for regex not to interpret it as a token so `\\n`. *as always the usual backslash*

# Grouping
*played around on a game I found online to see If I had fully grasped it, and came across an issue that deserves a part*.

You can group certain part of the expression using parentheses : for instance,

| match | aaaabcc | ![Success](https://regexone.com/cs/images/task_complete.png) |
| ----- | ------- | ------------------------------------------------------------ |
| match | aabbbbc | ![Success](https://regexone.com/cs/images/task_complete.png) |
| match | aacc    | ![Success](https://regexone.com/cs/images/task_complete.png) |
| skip  | a       |                                                              |
This was the exercise, and my regex is the following:
`a{2,}.|b+c+`, However, the `|` is causing the expression to be divided into two parts. However, i want it to only check for `.` or `b+`, and not `a{2,}.` or `b+c+`, to fix this you can use parentheses:
`a{2,}(.|b+)c+`.

the game is [here](https://regexone.com/lesson/kleene_operators?) btw.




