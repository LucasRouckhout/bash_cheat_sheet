# Bash Cheat Sheet

A bash cheat sheet with code listings for common tasks. This document is a cookbook. It does not explain why you do things
in depth it only shows you what to do. It is meant for people who already have some experience in bash and quickly need to
hack something together. 

The idea is that one can open this document on one monitor and open a text editor in the other one
and copy paste sollutions with some small adjustments over to get a working script that does something.


# Table of contents

- [Strings & File content](#strings---file-content)
  * [Reading a file line by line](#reading-a-file-line-by-line)
  * [Splitting a string into parts via a delimiter](#splitting-a-string-into-parts-via-a-delimiter)
- [Conditionals](#conditionals)
  * [Run a command conditionally (chaining)](#run-a-command-conditionally--chaining-)
- [The Find command](#the-find-command)

# Strings & File content

This section covers the manipulation and processing of strings and file contents.

## Reading a file line by line

```bash
FILE="filename"
while IFE= read line; do
	echo "$line"
done < "$FILE"
```

* You can possibly add a `-r` flag to the read command which will ignore escaping with `\` characters.
* You can choose the delimiter with `IFS="delim"` in this case the default delimiter of a newline will be used.

## Splitting a string into parts via a delimiter

 ```bash
 IFS=":" read first second rest <<< "$STRING"
 ```

 * You can chose the delimiter with `IFS="delim"`
 * The first word before the delimiter will be put in the `first` variable, the second in the `second` variable and so on.
 * If the amount of delimited strings is greater then the ammount of provided variables then the rest will of the string
 will be appended to the last provided variable. In this case the `rest` variable.

# Conditionals

## Run a command conditionally (chaining)

If you want to run a piece of code only when a logical condition is met do the following.

```bash
[[ condition ]] && echo "The condition was met"
```

Example, only read a file when it exists then print out all the lines.

```bash
[[ -a $FILE ]] && while read -r line; do echo $line; done < $FILE
```

These conditions can be chained. And will be checked in order.

```bash
[[ condition ]] && [[ condition2 ]] && echo "Some code"
```

__NOTE__: These are all syntactical suger for the plain old if statement. You can always use nested if statements
but they are less readable. So the above statement could be written as follows.

```bash
if [[ condition ]]; then
	if [[ condition1 ]]; then
		echo "Some code"
	fi
fi
```

# The Find command 

Find is not a shell builtin but a tool commonly used in scripts. You can find it by default on most UNIX environments.

```bash
find /dev -name 'sda*'
```

This will print out all the partitions of the disk linked to the device file `/dev/sda` and the device file itself.
A find command is a bit different from normal GNU tools. The first argument is the directory where you want to search.
Then a set of command called predicates follow. For each file in the directory find will evaluate all these predicates
in order and only files that produce true for all of them will be printed out. 

The above command only has one predicate `-name` which simply checks if the file name starts with `sda` (notice the use of the wildcard).
But you could add multiple predicates.

```bash
find /dev -name 'sda*' -name '*1'
```

This will print out all the files that start with `sda` and end with `1`. So only the first partition of `/dev/sda`.
The `-name` predicate is the simpelest one but there are a lot of them. Check `man find` for a listing of them. 
Some examples:

* `-regex` checks if the file matches the regex.
* `-empty` evaluates to true if the file is empty.

You can also negate the predicate by providing a `!` before the predicate. So to get only the partitions without the 
device file one can do something like this.


```bash
find /dev -name 'sda*' ! -name 'sda'
```

Which translates to: every file that starts with `sda` but not the file that matches `sda` exactly.

__NOTE__: The `-print` predicate always evaluates to true and prints out the files. This is default behavior on most machines
but you can specify another print behavior with predicates like `-print0` which does not print out a newline but adds a NULL
character. Or the predicate `-printf` which allows formatted printing.

## Find/list all the subdirectories of a directory

```bash
find /dev -type d -print
```

Using the `-type` predicate you can filter on the basis of file type. For a quick explanation on the syntax see 
introduction to this section. For a quick explanation on the syntax see the introduction to this section [The Find command](#the-find-command).


## Find/list only files of a certain type.

For example find/list all the sockets in a directory.

```bash
find /run -type s -print
```

Using the `-type` predicate you can filter on the basis of file type. 

For a quick explanation on the syntax see the introduction to this section [The Find command](#the-find-command).

## Find/list only files of a certain type.

For example find/list all the sockets in a directory.

```bash
find /run -type s -print
```

Using the `-type` predicate you can filter on the basis of file type. 

```
-type c
              File is of type c:

              b      block (buffered) special

              c      character (unbuffered) special

              d      directory

              p      named pipe (FIFO)

              f      regular file

              l      symbolic  link;  this is never true if the -L option or the -follow option is in effect, unless
                     the symbolic link is broken.  If you want to search for symbolic links when -L  is  in  effect,
                     use -xtype.

              s      socket

              D      door (Solaris)

              To  search  for more than one type at once, you can supply the combined list of type letters separated
              by a comma `,' (GNU extension).
```
