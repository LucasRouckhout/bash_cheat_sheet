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
