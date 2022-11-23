[Source](https://linux.die.net/abs-guide/)

# Chapter 3. Special Characters

Lines Beginning with `#` are comments
```bash
# This is a comment
```

`;` is used for command seperation
```bash
echo hello; echo there
```

`;;` terminator in a case option
```bash
case "$variable" in
abc)  echo "\$variable = abc" ;;
xyz)  echo "\$variable = xyz" ;;
esac
```

`.` is equivalent to `source`. 

While working with filenames `.` is the prefix for a hidden file.

When considering directory names `.` represents current working directory, and `..` represents parent directory.

In regex `.` will match a single character.

Use `"` for partial quoting and `'` for full quoting. Full quoting is stronger than the former.

The comma operator `,` links together series of arithmetic operations. All are evaluated but only the last one is returned.
```bash
let "t2 = ((a = 9, 15 / 3))"  # Set "a = 9" and "t2 = 15 / 3"
```

Use `\` to escape characters. `\x` has the same effect as of `'x'`.

`/` separates components of a filename. e.g. `home/foo/Makefile`.

`` ` `` backticks can be used to run commands inside scripts. 
```bash
CURR_DIR=`pwd`
```

`:` is the null command (NOP) and its exit status is always "true" (0).
```bash
:
echo $?   # 0
```
In combination with the redirection operator `>`, truncates a file size to zero without changing its permissions. If the file does not exist, creates it. Same for `>>`, just that it does not clear the file contents.
```bash
: > data.xxx   # File "data.xxx" now empty.
```
Can also be used to begin a comment, but not recommended as error checking will still apply.
Can also serve as field separator in some cases.
```bash
bash$ **echo $PATH**
/usr/local/bin:/bin:/usr/bin:/usr/X11R6/bin:/sbin:/usr/sbin:/usr/games
```

`!` will negate a test or the exit status of the command  to which it is applied.

`*` in globbing syntax serves as a wildcard for filename expansion. In regex it represents zero or more occurrence of preceding pattern. In arithmetic expressions it is the usual multiplication operator, while `**` represents exponentiation.

`?` in regex and globbing represents a single character wildcard. In parameter substitution expressions it tests whether a variable has been set.

`$` 

Variable substitution
```bash
var1=5
echo $var1     # 5
```
EOL in regex

`${}`

Parameter substitution

`$*`, `$@`

Positional parameters

`$?`

Exit status

`$$`

Process ID

`()`

Command group. A listing of commands within parenthesis starts a subshell. Variables inside parentheses, within the subshell, are not visible to the rest of the script.
```bash
a=123
( a=321; )	      

echo "a = $a"   # a = 123
# "a" within parentheses acts like a local variable.
```
Array initialization
```bash
Array=(element1 element2 element3)
```