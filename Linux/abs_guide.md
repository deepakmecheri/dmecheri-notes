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

`{xxx,yyy,zzz,...}`

Brace expansion
```bash
cat {file1,file2,file3} > combined_file
# Concatenates the files file1, file2, and file3 into combined_file.


cp file22.{txt,backup}
# Copies "file22.txt" to "file22.backup"
```

`{a..z}`
Extended Brace expansion
```bash
echo {a..z} # a b c d e f g h i j k l m n o p q r s t u v w x y z
# Echoes characters between a and z.

echo {0..3} # 0 1 2 3
# Echoes characters between 0 and 3.
```

`{}`

Block of code [curly brackets]. Also referred to as an inline group, this construct, in effect, creates an anonymous function (a function without a name). However, unlike in a "standard" function, the variables inside a code block remain visible to the remainder of the script.
```bash
a=123
{ a=321; }
echo "a = $a"   # a = 321   (value inside code block)
```

```bash
#!/bin/bash
# rpm-check.sh

#  Queries an rpm file for description, listing,
#+ and whether it can be installed.
#  Saves output to a file.
# 
#  This script illustrates using a code block.

SUCCESS=0
E_NOARGS=65

if [ -z "$1" ]
then
  echo "Usage: `basename $0` rpm-file"
  exit $E_NOARGS
fi  

{ # Begin code block.
  echo
  echo "Archive Description:"
  rpm -qpi $1       # Query description.
  echo
  echo "Archive Listing:"
  rpm -qpl $1       # Query listing.
  echo
  rpm -i --test $1  # Query whether rpm file can be installed.
  if [ "$?" -eq $SUCCESS ]
  then
    echo "$1 can be installed."
  else
    echo "$1 cannot be installed."
  fi  
  echo              # End code block.
} > "$1.test"       # Redirects output of everything in block to file.

echo "Results of rpm test in file $1.test"

# See rpm man page for explanation of options.

exit 0
```

> **Note**: Unlike a command group within `()`, a code block enclosed by `{}` will not normally launch a subshell.

`[ ]`, `[[ ]]`

Test expression

`[]`

Array element
```bash
Array[1]=slot_1
echo ${Array[1]}
```

`(( ))`

Integer expansion

`>` `&>` `>&` `>>` `<` `<>`

Redirection and process substitution

`<<`

Redirection in here document

`<<<`

Redirection used in a here string

`<`, `>`

ASCII comparison

`\<`, `\>`

Regex word boundary
```
bash$ grep '\<the\>' textfile
```

`|`

Send stdout of one command to stdin of the next
```bash
echo ls -l | sh
```

`>|`

Force redirection

`||`

Logical OR

`&`

Run job in background

```
bash$ sleep 10 &
[1] 850
[1]+  Done                    sleep 10
```

`&&`

Logical AND

`-`

option/prefix
```bash
ls -al
```
```bash
if [ $file1 -ot $file2 ]
then
  echo "File $file1 is older than $file2."
fi
```

redirection from/to stdin or stdout 
```bash
bash$ echo "whatever" | cat -
whatever 
```

Note that in this context the "-" is not itself a Bash operator, but rather an option recognized by certain UNIX utilities that write to stdout, such as tar, cat, etc.

Previous working directory
```bash
cd -
```

Arithmetic minus

`=`

Assignment
```bash
a=28
echo $a   # 28
```

`+`

Arithmetic addition

Option flag to enable certain options

`%`

Modulo operator

`~`

Home directory

`~+`

Equivalent to `$PWD`

`~-`

Equivalent to `$OLDPWD`

`^`

Beginning of line in regex

### Control Characters

`Ctl-F`/`Ctl-B`

Move forwards and backwards in terminal

`Ctl-C`

Break. Terminate foreground job.

`Ctl-D`

Log out of shell

EOF - also terminates input from *stdin*

`Ctl-G`

BEL - beep!

`Ctl-H`

Destructive backspace

`Ctl-I`

Horizontal tab

`Ctl-J`

Newline

`Ctl-K`

Vertical tab

`Ctl-L`

Formfeed (clear the terminal screen). In a terminal, this has the same effect as the clear command. When sent to a printer, a Ctl-L causes an advance to end of the paper sheet.

`Ctl-M`

Carriage return.

`Ctl-Q`

Resume stdin

`Ctl-S`

Freeze stdin

`Ctl-U`

Erase to beginning of line

`Ctl-W`

Erase backwards till whitespace

`Ctl-Z`

Pause foreground job


