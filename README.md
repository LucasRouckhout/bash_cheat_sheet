# Bash Cheat Sheet

A bash cheat sheet with code listings for common tasks.

## Reading a file line by line

```bash
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

## Find command 

This is a special one and technically does not belong here sinds the `find` command is not a shell-buitlin.
But because it is so powerfull and used a lot in scripts a quick introduction is in order. The most basic usage of
find would be to search for files in a directory with a search pattern.

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
