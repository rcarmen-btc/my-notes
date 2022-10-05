# Learn Bash Scripting by Building Five Programs

#BashScripting #SQL

### Key words | Questions

### Notes

## Help

```bash
help <command/expression>
```

## Commenting

This is fun. You can create a multiline comment like this:

```bash
# comment

: '
  comment here
  more comment here
'
```

## Which

```bash
which bash 
```

## Shebang

That's the absolute path to the bash interpreter. You can tell your program to use it by placing a shebang at the very top of the file like this: `#!<path_to_interpreter>`. Add a shebang at the very top of your file, the one you want looks like this:

```bash
#!/bin/bash

#./filename - execute
```

## Vars

```bash
VAR="value"

echo $VAR
```

The question was printed. Next, you want to be able to accept input from a user. You can do that with read like this: read VARIABLE_NAME. This will get user input and store it into a new variable. After you print the question, use read to get input and store it in a variable named NAME.

```bash
read $NAME # read from input 
```

## Args

```bash
echo $* # all arguments 

echo $1 # first argument
```

## Conditions

```bash
if [[ CONDITION ]]
then
  STATEMENTS
fi
```

You can compare integers inside the brackets (`[[ ... ]]`) of your if with `-eq` (equal), `-ne` (not equal), `-lt` (less than), `-le` (less than or equal), `-gt` (greater than), `-ge` (greater than or equal). Change your if condition to check if your first argument is less than 5.

```bash
if [[ $1 == arg1 ]]
then
  echo true
else
  echo false  
fi

# ===

if [[ $1 -lt 5 ]]
then
  echo true
else
  echo false  
fi
```

### && and ||

```bash
[[ -x countdown.sh && 5 -le 4 ]]; echo $?

[[ -x countdown.sh || 5 -le 4 ]]; echo $?
```

## Loops

### For

```
for (( i = 10; i > 0; i-- ))
do
  echo $i
done
```

### While

The menu showed that you can make a while loop like this:

```bash

while [[ CONDITION ]]
do
  STATEMENTS
done
```

It worked. Instead of printing the content, you can pipe that output into a while loop so you can go through the rows one at a time. It looks like this:

```bash
cat courses.csv | while read MAJOR COURSE
do
  <STATEMENTS>
done
```

Each new line will be read into the variables, `MAJOR` and `COURSE`. Add the above to your `cat` command. In the `STATEMENTS` area, use `echo` to print the `MAJOR` variable.

It's looping, but the `MAJOR` variable is only being set to the first word. There's a default `IFS` variable in bash. IFS stands for "Internal Field Separator". View it with `declare -p IFS`.

```bash
#!/bin/bash

# Script to insert data from courses.csv and students.csv into students database

cat courses.csv | while IFS="," read MAJOR COURSE
do
    echo $MAJOR
done
```

## Expression

Execute conditional command.

When the `==' and` !=' operators are used, the string to the right of
the operator is used as a pattern and pattern matching is performed.
When the `=~' operator is used, the string to the right of the operator
is matched as a regular expression.

```bash
test [expression]
```

## Working with files

Evaluate conditional expression:

```bash
help test
```

Try one of the file operators. The first one on the list checks if a file exists. 

Type `[[ -a countdown.sh ]]; echo $?` in the terminal to see if your file exists.

## Env

View all variables in the shell with `declare -p`. `-p` stands for print

```bash
codeally@b0a8f03fa55b:~/project$ declare -p
declare -- BASH="/usr/bin/bash"
declare -r BASHOPTS="checkwinsi..."
codeally@b0a8f03fa55b:~/project$ printenv
...
```

## (( ... ))

The double parenthesis performed the calculation, changing the value of `I` from `0` to `1`. Enter `help let` in the terminal to see the operators you can use with the double parenthesis.

It should have printed `11` for the value of `I`. Using the double parenthesis like you have been is good for changing variable values or making comparisons. It makes the calculation in place and provides no output. If you want to make a calculation and do something with the result, add a `$` in front like this: `$(( ... ))`. Type `$(( I + 4 ))` in the terminal to see what happens.

So, as a reminder, `(( ... ))` will perform a calculation or operation and output nothing. `$(( ... ))` will replace the calculation with the result of it.

## let

```bash
codeally@b0a8f03fa55b:~/project$ help let
let: let arg [arg ...]
      Evaluate arithmetic expressions.

      Evaluate each ARG as an arithmetic expression.  Evaluation is done in
      fixed-width integers with no check for overflow
			 ........
```

## Arrays

This program will have an array of responses. One will be printed randomly after a user inputs a question. Practice first 😄 In the terminal, create an array like this: `ARR=("a" "b" "c")`

Each variable in the array is like any other variable, just combined into a single variable. In the terminal, print the second item in the array with `echo ${ARR[1]}`. Note that the first item would be index zero.

If you recall, you were able to print all the arguments to your [countdown.sh](http://countdown.sh/) script with echo `$*`. echo `$@` would have worked as well. Similarly, you can use the * or @ to print your whole array. In the terminal, use echo to print all the items in your array. `echo ${ARR[*]}`

## Function

```bash
function: function name { COMMANDS ; } or name () { COMMANDS ; }
       Define shell function.
       Create a shell function named NAME. 
       .........
```

It looks like you can create a function like this:

```bash
FUNCTION_NAME() {
  STATEMENTS
}

FUNCTION_NAME
```

Call your function by putting the name of it below where you create it. No `$` needed. Make sure the reponse you are printing is at the bottom of the file.

## Until

```bash
codeally@b0a8f03fa55b:~/project$ help until
until: until COMMANDS; do COMMANDS; done
    Execute commands as long as a test does not succeed.
```

The `until` loop is very similar to the `while` loop you used. It will execute the loop until a condition is met. Here's an example:

```bash
until [[ CONDITION ]]
do
  STATEMENTS
done
```

## Type

```bash
codeally@b0a8f03fa55b:~/project$ help type
type: type [-afptP] name [name ...]
    Display information about command type.
    
    For each NAME, indicate how it would be interpreted if used as a
    command name.
```

## Scripts and psql

You used the `psql` command to log in and interact with the database. You can use it to just run a single command and exit. Above your loop, add a `PSQL` variable that looks like this: `PSQL="psql -X --username=freecodecamp --dbname=students --no-align --tuples-only -c"`. This will allow you to query your database from your script. The important parts are the `username`, `dbname`, and the `-c` flag that is for running a single command and exiting. The rest of the flags are for formatting.

Now, you can query your database using the `PSQL` variable like this: `$($PSQL "<query_here>")`. Below the `get major_id` comment in your loop, create a `MAJOR_ID` variable. Set it equal to the result of a query that gets the `major_id` of the current `MAJOR` in the loop. Make sure to put your `MAJOR` variable in single quotes.

<aside>
📌 **SUMMARY:**

</aside>