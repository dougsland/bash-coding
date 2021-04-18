- [bash-coding](#bash-coding)
  * [echo](#echo)
    + [Supress new line](#Supress-new-line)
    + [Escape sequence](#Escape-sequence)
  * [comments](#comments)
  * [read](#read)
  * [Variables](#variables)
  * [Debugging](#debugging)
  * [Arguments](#arguments)
    + [argc argv](#argc-argv)
    + [Argument Count](#Argument-Count)
    + [Default Values Argument](#Default-Values-Argument)
    + [Getopt](#getopt)
  * [Quotes](#quotes)
    + [double quotes](#double-quotes)
    + [single quotes](#single-quotes)
  * [array](#array)
    + [loop into array](#loop-into-array)
  * [curl](#curl)
    + [Download and set the output to a file](#download-and-set-the-output-to-a-file)
  * [if](#if)
    + [square or double square](#square-or-double-square)
    + [Check if file is executable](#Check-if-file-is-executable)
    + [Check if file is writable](#Check-if-file-is-writable)
    + [Check if file is readable](#Check-if-file-is-readable)
    + [Check if directory exists](#Check-if-directory-exists)
    + [Check if command works](#Check-if-a-command-works)
    + [Check if file exists](#Check-if-file-exists)
    + [Check if variable is set](#Check-if-variable-is-set)
    + [Check if variable length is greater than zero](#Check-if-variable-length-is-greater-than-zero)
  * [if - elif - else](#if---elif---else)
  * [if - strings](#if---strings)
  * [if - Numeric operators](#if---Numeric-operators)
    + [less than](#less-than)
    + [greater than](#greater-than)
    + [equal](#equal)
  * [if - combining tests](#if---combining-tests)
  * [Math in Bash](#Math-in-Bash)
  * [for](#for)
    + [List files in the current dir](#List-files-in-the-current-dir)
    + [Nested Loops](#Nested-loops)
  * [functions](#functions)
  * [grep](#grep)
  * [while](#while)
  * [case](#case)
  * [IFS](#IFS)
  * [sed](#sed)
  * [awk](#awk)
  * [date](#date)
    + [Current date](#Current-date)
    + [Current date and time](#Current-date-and-time)
  * [find](#find)
    + [find files by extension](#find-files-by-extension)
  * [tr](#tr)
    + [Generate random name](#Generate-random-name-with-tr)
  * [Regex](#regex)
    + [Operators for Regex](#Operators-for-regex)
  * [Shellcheck](#shellcheck)
    + [Disable Shellcheck](#Disable-Shellcheck)
  * [Operators](#Operators)
  * [Files](#files)
    + [Reading line by line](#reading-line-by-line)
  * [Geopt](#getopt)
  * [Colors](#Colors)
  * [Menu](#Menu)
- [Books](#books)
  
# bash-coding
## comments

```
#!/usr/bin/env bash
# My comment 1
# My comment 2
ls
```

## echo
### Supress new line
```
echo -n "Test String"
```
### Escape Sequence
Users can use \t \n or any other escape using **-e**
```
echo -e "Test String\n"
```
## read
The read will capture the input from user. If user do not provide the argument to read. The input will be captured
in the $REPLY var.

Options:  
**-p**  Display prompt on standard error, without a trailing newline, before attempting to read any input.  
The prompt is displayed only if input is coming from a terminal.

**-n** read returns after reading nchars characters rather than waiting for a complete line of input,
but honors a delimiter if fewer than nchars characters are read before the delimiter.

**-s** Silent mode.  If input is coming from a terminal, characters are not echoed.  

Code:  
```
#!/usr/bin/env bash

read -p "What's your name?" myanswer
echo "${myanswer}"

```

Code:  
```
#!/usr/bin/env bash

echo "Hello there!"
read -n1 -p "Press enter to continue"

```
Code:  
```
#!/usr/bin/env bash

echo "What's your name?"
read
echo "${REPLY}"
```

Code:  
```
#!/usr/bin/env bash

echo "What's your name?"
read myanswer
echo "${myanswer}"
```

## Variables
Use `var=value` please note **there is not space between varname=value**
```
#!/usr/bin/env bash
car="Fiesta"
motor=1.0
year=1970

echo "${car}"
echo "${motor}"
echo "${year}"

```

## Debugging
bash provides **-x** and **-v** for verbose/debug mode

```
$ bash -x script.sh
+ car=Fiesta
+ motor=1.0
+ year=1970
+ echo Fiesta
Fiesta
+ echo 1.0
1.0
+ echo 1970
1970
```

Verbose:

```
$ bash -v script.sh
#!/usr/bin/env bash
car="Fiesta"
motor=1.0
year=1970

echo "${car}"
Fiesta
echo "${motor}"
1.0
echo "${year}"
1970
```

## Arguments
```
$0 - Name of the shell script  
$1 - First argument, **$2** second argument, $3 etc.  
$* - All arguments  
$# - The argument count  
```

Example 1:
```
$ sh example foo 1
The script is named as: example
Argument one is: foo
Argument two is: 1
Total number of arguments: 2
```

```
#!/usr/bin/env bash

echo "The script is named as: ${0}"
echo "Argument one is: ${1}"
echo "Argument two is: ${2}"
echo "Total number of arguments: $#"
```
### argc argv
  Example:
  ```
  $ ./example foobar myopt
  foobar
  myopt
  ```

  Code:
  ```
  #!/usr/bin/env bash
  
  function main(){
      for opt in $@; do
          echo "${opt}"
      done;
  }

  main $@
  ```
### Argument Count
Code:
```
if [ ! $# -eq 1 ]
  then
    echo "You must provide one argument to the script"
    echo "Usage $0 [OPTIONS]...
    exit 1
fi
```

### Getopt
Example 2:  
```
#!/usr/bin/env bash

usage () {
    echo "usage message..."
}

options=$(getopt -o ug --long createuser:,creategroup: -- "$@")
[ $? -eq 0 ] || {
    usage
    exit 1
}

eval set -- "$options"
while true; do
    case "$1" in
    -g | --creategroup)
        shift
        OPTION="$1"
        ;;
    -u | --createuser)
        shift
        OPTION="$1"
        ;;
    --)
        shift
        break
        ;;
    esac
    shift
done

if [ -n "$OPTION" ]; then
    echo "Option is $OPTION"
else
    usage
    exit 1
fi

exit 0
```

## Math in Bash
For mathematical expression, use must use `(( ))` expression.

```
$ echo $(( 1+2 ))
3

$ foobar=$((1+4))
$ echo "${foobar}"
5

$ count=1
$ ((count++))
$ echo $count
2

$ ((count--))
$ echo $count
1

$ (( count > 1 )) && echo "count is greater then 0"

```

## Default Values Argument
In case users would like to set default value in case argument {$1} is not provided, here example:

```
#!/usr/bin/env bash

hostname=${1-default.value.org}
echo "${hostname}"
```

Example 1 (providing argument):
```
$ ./test.sh myhost.com.br
Hostname is: myhost.com.br
```

Example 2 (no arguments):
```
$ ./test.sh 
Hostname is: default.value.org
```

## quotes
### double quotes
When users use double quotes, bash **will expand the variable**, example:
```
#!/usr/bin/env bash

MYVAR="good morning"
echo "${MYVAR}"
```

Output:
```
good morning
```

If users would like to **not expand** the variable, they can use the ```\${MYVAR}```
```
#!/usr/bin/env bash

MYVAR="good morning"
echo "\${MYVAR}"
```

Output:
```
${MYVAR}
```

### single quotes
With single quotes, bash **won't expand any variable**, we call it as "raw mode"

```
#!/usr/bin/env bash

MYVAR="good morning"
echo '${MYVAR}'
```

Output:
```
${MYVAR}
```

## array
  Example:
  ```
  cars=("fusca" "ferrari" "s10")
  numbers=(one two three four)
  colors=("azul" "amarelo" "azul" "rosa") 
  ```
### loop into array
  Code:
  ```
  #!/usr/bin/env bash

  manifests=(https://foo.yaml https://bar.yaml https://foohosts.yaml)
  for manifest in "${manifests[@]}"; do
    echo "Applying ${manifest}..."
    kubectl apply -f "${manifest}"
  done
  ```

## if
### square or double square
Instead of use the `test` command bash support conditional tests via brackets. 

instead of:
```
$ test -f /etc/hosts -a -r /etc/hosts
```

use:

```
$ [ -f /etc/hosts -a -r /etc/hosts ]
```

However, bash also provides `[[` allow users to do more advanced conditions testing.
It also supported to others shells like: `KornShell` and `zsh`. Unlike the single
bracket, this is not a command but a keyword. The fact that `[[` is not a command
is significant where white space is concerned. As a keyword, `[[` parses its arguments
before bash expands them. As such, a single parameter will always be represented as a single argument.
Even though it goes against best practice, `[[` can alleviate some of the issues associated with white
space within parameter values. 

```
if [[ -f ${FILENAME} && -r ${FILENAME} ]] && cat ${FILENAME}
```

**Pattern matching**

Example how to use regular expression:
```
if [[ "${FILENAME}" = \*.lib ]] && cp "${FILENAME}" libdir/
```

For users missing the second `o` in book word:
```
if [[ ${USERINPUT}" =~ bo?k ]] ; then
    echo "book..."
fi
```

### Check if file is executable
Code:  
```
#!/usr/bin/env bash

if [[ -x /tmp/foobar ]]; then
    echo "File exists and is executable"
else
    echo "File not executable"
fi
```

### Check if file is writable
Code:  
```
#!/usr/bin/env bash

if [[ -w /tmp/foobar ]]; then
    echo "File exists and is writable"  
else
    echo "File not exist or not writable"  
fi
```

### Check if file is readable
Code:  
```
#!/usr/bin/env bash

if [[ -r /root/foobar ]]; then
    echo "File exists and readable"
else
    echo "File do not exist or not readable"
fi
```

### Check if directory exists
Code:  
```
if [[ -d /tmp ]]; then
    echo "Directory exists"
else
    echo "Directory not found"
fi
```

### Check if file exists
 Code:
 ```
 if [[ -e /etc/resolv.conf ]]; then
    echo "File exists"
 else
    echo "File not found"
 fi
 ```
 
 File Exists and it's a regular file  
 Code:
 ```
 #!/usr/bin/env bash
  
 if [ -f /tmp/doug ]; then
    echo 'exists'
 else
    echo 'file not found'
 fi
 ```
### Check if a command works
 Code:
 ```
 #!/usr/bin/env bash

 cd /foobar &> /dev/null
 if [ $? -ne 0 ]
 then
    echo "failed"
 else
    echo "success"
 fi
 ```

Code:
```
 #!/usr/bin/env bash
 
 if myfoobar_command &> /dev/null ; then
   echo "worked"
 else
   echo "Command failed"
   exit 1
 fi
 ```

## Check if variable is set
Basically it checks if variable length is zero.

Code:  
```
#!/usr/bin/env bash

foo=""

if [ -z "${foo}" ]; then
   echo "foo variable is not set"
fi
```

## Check if variable length is greater than zero

Code:  
```
#!/usr/bin/env bash

bar="foobar"

if [ -n "${bar}" ]; then
   echo "bar length is greater than 0"
fi
```
## if - elif - else
```
#!/usr/bin/env bash

if [  ]
then
    echo "if.."
elif [  ]
then
    echo "elif.."
else
    echo "else.."
fi
```
## if - strings
|  Operator                   | Description                          |
|-----------------------------|--------------------------------------|
|  [ "${str1}" = "${str2}" ]  | Check if str1 is equal str2          |
|  [ "${str1}" != "${str2}" ] | Check if str1 is different than str2 |
|  [ "${str1}" \< "${str2}" ] | Check if str1 is less than str2      |
|  [ "${str1}" \> "${str2}" ] | Check if str1 is greater than str2   |

```
#!/usr/bin/env bash

str1="foo"
str2="bar"

if [ "${str1}" = "${str2}" ] # Check if string is equal
then
  echo "str1: ${str1} is equal str2: ${str2}"
elif [ "${str1}" != "${str2}" ] # Check if the string is different
then
  echo "str1: ${str1} is different than str2: ${str2}"
else 
  echo "None of the above"
fi
```

## if - combining tests
Users can use `&&` or `||` to combine validations
```
#!/usr/bin/env bash

dir_name="/tmp"

if [ -z "${dir_name}" ] && [ -d "${dir_name}" ]
then
   echo "The var dir_name is set and the directory exists"
fi
```

## if - Numeric operators
### less than

Code:  
```
#!/usr/bin/env bash

VALUE=1
MAX=5

if [[ "${VALUE}" -lt "${MAX}" ]]; then
    echo "The value ${VALUE} is less than $MAX"
fi
```
### greater than
Code:  
```
#!/usr/bin/env bash

VALUE=10
MAX=5

if [[ "${VALUE}" -gt "${MAX}" ]]; then
    echo "The value ${VALUE} is greater than $MAX"
fi
```
### equal
Code:  
```
#!/usr/bin/env bash

VALUE=10
MAX=10

if [[ "${VALUE}" -eq "${MAX}" ]]; then
    echo "The value ${VALUE} is equal max: $MAX"
fi
```

## for

Users can use the `for` command to loop into a variable, array, or multiple values.  
There are also `break` command to stop the interation or `continue` to continue the interaction.

```
#!/usr/bin/env bash

for user in douglas joanna; do
        echo $user
done
```

## functions

Example 1:
```
foobar() {
   <code-to-execute>
}
```

Example 2:
```
function foobar() {
   < code to execute >
}
```

The keyword function is deprecated for portability with the Portable Operating System Interface (POSIX)  
specification, but it is still used by some developers.

When using arguments use ${1} for the first argument, ${2} for the second etc.

Example 3 (Using arguments):
```
# Calling function example:
foobar "juju"

# Function definiton
function foobar() {
    echo "${1}"
}
```

Example 4:
```
# Calling function with array
arr=(1 2 3)
foobar "${arra}"

# Function definition
function foobar() {
    myarr=$@
    echo "the array ... ${myarr[*]}"
}
```

To `return` specific value from a function use the keyword **return**

## grep

Example 1 (Show 2 additiona lines after match):
```
$ cat /etc/passwd | grep -A2 douglas
```

Example 2:
```
CPU_CORES=$(grep -c name /proc/cpuinfo)
if (( CPU_CORES < 4 )) ; then
    echo "A minimum CPU is 4"
    exit 1
fi
```
## while
Count down example:
```
#!/usr/bin/env bash

COUNT=5
while (( COUNT >= 0 )) ; do
    echo $COUNT
((COUNT--))
done;
```

### List files in the current dir
```
#!/usr/bin/env bash

for filename in *; do
        echo "${filename}"
done
```

### Nested Loops
```
#!/usr/bin/env bash
for user in bob jack hacker douglas; do
    for data in $(cat /etc/passwd | grep ${user}) $(cat /etc/group | grep ${user}); do
       echo  ${user} ${data}
    done 2> /dev/null
done
```

## sed

- Replace all occurencies of `foobar` with `the great foobar`:
```
$ sed 's/foobar/the great foobar/g' original_file > new_file
```

- To save the changes in a new file:
```
$ sed 's/foobar/the great foobar/w NEWFILE' original_file
```

- Replace the first two occurences only
```
$ sed 's/foobar/the great foobar/2' original_file
```

- Replace from line 3 to 5
```
$ sed '3,5s/foobar/the great foobar/' original_file
```

- Replace from line 2 until the end of file
```
$ sed '2,$s/foobar/the great foobar/' original_file
```

- Replace only line 2
```
$ sed '2s/foobar/the great foobar/' original_file
```

- Delete line 3
```
$ sed '3d' original_file
```

- Delete line 3 until 5
```
$ sed '3,5d' original_file
```

- Delete from the line 2 until the end of file
```
$ sed '2,$d' original_file
```

- Insert text in the second line
```
$ sed '2i\foobar' original_file
```

- Append text
```
$ sed '2a\insert text' original_file
```

- Change second line
```
$ sed '2c\new line to be add' original_file
```

- Transform
Replace a number or a letter in the entire file
```
$ sed 'y/zeze/ZEZE/' original_file
```

- Multiple sed commands
```
$ sed -e 's/First/Title; s/Second/Job/' original_file
```

or

```
$ sed -e '
  s/First/Title/
  s/Second/Job/' original_file
```

## IFS
By default the IFS variable has the value of (one space). However, you can use
newline or TAB

Example 1 (using space):

```
#!/usr/bin/env bash
file=myfile.txt
for var in $(cat $file)
do
    echo "${var}"
done
```

Now using IFS with `newline`

```
#!/usr/bin/env bash
file=myfile.txt
IFS=$"\n"
for var in $(cat $file)
do
    echo "${var}"
done
```


## case
```
#!/usr/bin/env bash

read -p "Please provide your lastname:" LASTNAME

case "${LASTNAME}" in

  Silva)
    echo "Found Silva in the database"
    ;;

  Schilling | schilling)
    echo "Found Schilling in the database"
    ;;

  Sal)
    echo "Found Sal"
    ;;

  *)
    echo "unknown lastname"
    ;;
esac
```
## tr
### Generate random name with tr
  Random name with 6 chars
  Example:
  ```
  $ tr -dc A-Za-z0-9 < /dev/urandom | head -c 6 ; echo ''
  UZr6F4
  ```

## curl
### Download and set the output to a file
  Example:
  ```
  $ curl -o /path/filename https://url.name
  ```

## date
### Current date 
  Example:
  ```
  $ echo $(date '+%F')
  2021-03-19
  ```

### Current date and time
  Example:
  ```
  $ echo $(date +"%m-%d-%Y-%k:%M:%S")
  03-19-2021-10:26:43
  ```

## find
### find files by extension
  Example:
  ```
  find . -type f -name '*.sh'
  ```

## Regex
### Operators for regex

```
  ^ Matches the beginning of a string
  $ Matches the end of string
  . Matches any character exactly once
  + Matches previous character once or more often
  * Matches previous character zero times or mode often
  ? Matches previous character zero times or more often
  [abc] Matches one of characters in the list
  [^abc] Doesn't match one of the characters in the list
  [a-c] Matches all characters from a to c
  [^a-c] Doesn't match the characters from a to c
```

## AWK

- Variables

  `FIELDWIDTHS`: Specifies the field with  
  `RS`: Specifies the record separator  
  `FS`: Specifies the field separator  
  `OFS`: Specifies the output separator, which is a space by default  
  `ORS`: Specifies the output separator  
  `FILENAME`: Holds the processed file name  
  `NF`: Holds theline being processed  
  `FNR`: Holds the record which is processed  
  `IGNORECASE`: Ignores character case  

- Hello World
```
$ awk 'BEGIN { print "Hello World!" }'
```

- Display content from a file
```
$ awk ' { print } ' /etc/group
```

or equivalent

```
$ awk '{ print $0 }' /etc/group
```

- Display splitting by `:` (field one)
```
$ awk -F":" '{ print $1 }' /etc/group
```

- Display content and in the end show total number

BEGIN shows the start and parsing with `Field Separator` or `FS` we will use `:`  
and print the firt field. We will use `END` and later total  
`Number of Record` or `NR`. 
```
$ awk ' BEGIN { FS=":" } { print $1 } END { print NR } ' /etc/group
```

- User defined variables
```
$ awk '
BEGIN {
var="Hello world"
print var
}'
```

- Sum example
```
$ awk '
BEGIN {
var1=2
var2=3
var3=var1+var2
print var3
}'
```

```
$ awk '
BEGIN {
str1="Welcome "
str2="To shell scripting"
str3=str1 str2
print str3
}'
```

- if
Example of processing a file and multiple
```
$ cat myfile
50
30
80
70
20
90

$ awk '{
if ($1 < 50)
{
x = $1 * 2
print x
} else 
{
x = $1 * 3
print x
}}' myfile
```

- while
```
$ cat myfile
50
30
80
70
20
90

$ awk '{
total = 0
i = 1
while (i < 4)
{
total += $i
i++
}
mean = total / 3
print "Mean value:",mean
}' myfile
```

- for
```
$ cat myfile
50
30
80
70
20
90

$ awk ' {
total = 0
for (var = 1; var < 4; var++)
{
total += $var
}
mean = total / 3
print "Mean value:",mean
}' myfile
```

## Special vars
Check if command return succeds or not. 

**0**    - means worked  
**!= 0** - command failed
```
$ ls
$ echo $?
```
## Shellcheck
ShellCheck is a tool that gives warnings and suggestions for bash/sh shell scripts

Installing on Fedora:
```
$ sudo dnf install ShellCheck -y
```
### Disable Shellcheck
```
repo="https://github.com/foobar"
git clone "${repo}" 1> /dev/null
# shellcheck disable=SC2181
if [ $? -ne 0 ]
then
    echo "failed to mount nfs server"
fi
```
## Operatos
command2 will only execute if command1 fail.
```
$ command1 || command2
```

command2 will execute only if command1 sucessds
```
$ command1 && command2
```

## Files
### Reading line by line
```
#!/usr/bin/env bash

while read line
do
    echo "${line}"
    sleep 1
done < fedora-latest.yaml
```

## Colors
Example of showing colors in bash:
```
#!/usr/bin/env bash

colors=(
	''
	'BLACK:\t\033[0;30m\t\tTesting String \033[0m'           # Black
	'BLACK BOLD:\t\033[1;30m\tTesting String \033[0m'        # Black Bold
	'BLACK UNDERLINE:\t\033[4;30m\tTesting String \033[0m'   # Black Underline
	'BLACK BACKGROUND:\033[40m\tTesting String \033[0m'      # Black Background
	''

	'RED:\t\033[0;31m\t\tTesting String \033[0m'             # Red
	'RED BOLD:\t\033[1;31m\tTesting String \033[0m'          # Red Bold
	'RED UNDERLINE:\t\033[4;31m\tTesting String \033[0m'     # Red Underline
	'RED BACKGROUND:\t\033[41m\tTesting String \033[0m'      # Red Background
	''

	'GREEN:\t\033[0;32m\t\tTesting String \033[0m'           # Green
	'GREEN BOLD:\t\033[1;32m\tTesting String \033[0m'        # Green Bold
	'GREEN UNDERLINE:\t\033[4;32mTesting String \033[0m'     # Green Underline
	'GREEN BACKGROUND:\t\033[4;42mTesting String \033[0m'    # Green Background
	''

	'YELLOW:\t\t\033[0;33m\tTesting String \033[0m'          # Yellow
	'YELLOW BOLD:\t\033[1;33m\tTesting String \033[0m'       # Yellow Bold
	'YELLOW UNDERLINE:\t\033[4;33mTesting String \033[0m'    # Yellow Underline
	'YELLOW BACKGROUND:\t\033[43mTesting String \033[0m'     # Yellow Background
	''

	'BLUE:\t\033[0;34m\t\tTesting String \033[0m'            # BLUE
	'BLUE BOLD:\t\033[1;34m\tTesting String \033[0m'         # BLUE Bold
	'BLUE UNDERLINE:\t\t\033[4;34mTesting String \033[0m'    # BLUE Underline
	'BLUE BACKGROUND:\t\033[44mTesting String \033[0m'       # BLUE Background
	''

	'PURPLE:\t\t\033[0;35m\tTesting String \033[0m'          # PURPLE
	'PURPLE BOLD:\t\033[1;35m\tTesting String \033[0m'       # PURPLE Bold
	'PURPLE UNDERLINE:\t\033[4;35mTesting String \033[0m'    # PURPLE Underline
	'PURPLE BACKGROUND:\t\033[45mTesting String \033[0m'     # PURPLE Background
	''

	'CYAN:\t\033[0;36m\t\tTesting String \033[0m'            # CYAN
	'CYAN BOLD:\t\033[1;36m\tTesting String \033[0m'         # CYAN Bold
	'CYAN UNERLINE:\t\033[4;36m\tTesting String \033[0m'     # CYAN Underline 
	'CYAN BACKGROUND:\033[46m\tTesting String \033[0m'       # CYAN Background 
	''

	'WHITE:\t\033[0;37m\t\tTesting String \033[0m'           # WHITE
	'WHITE BOLD:\t\033[1;37m\tTesting String \033[0m'        # WHITE Bold
	'WHITE BOLD:\t\033[4;37m\tTesting String \033[0m'        # WHITE Underline
	'WHITE BACKGROUND:\033[47m\tTesting String \033[0m'      # WHITE Background
	''
)

for color in "${colors[@]}"; do
	 echo -e "${color}"
done
```

## Menu
```
#!/usr/bin/env bash
while true
do
    echo "Choose one item from the list"
    echo "a) Backup"
    echo "b) Calendar"
    echo "c) Exit"
    read -sn1
    case "${REPLY}" in
        a) echo "User selected A"
        ;;
        b) echo "User selected B"
        ;;
        c) echo "User selected C"
    esac
    read -n1 -p "Press any key to continue"
done
```
# Books
[Mastering Linux Shell Scripting: 2nd Edition](https://read.amazon.com/kp/embed?asin=B07BWLL95K&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_TC57NN3M2674JVF0C2YF&tag=dougsland)
