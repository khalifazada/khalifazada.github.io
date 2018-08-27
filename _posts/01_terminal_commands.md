---
layout: post
title: Useful Terminal Commands Cheatsheet
---

![Command Line](https://cdn1.macworld.co.uk/cmsdata/features/3608274/Terminalicon2_thumb800.png)

# Terminal Commands
___
[TOC]
___

## Basics
### Working with Directories

- **`pwd`** 
Prints the working directory

- **`cd`**`/path/to_directory`
Switches to the specified directory

- **`whoami`**
Checks the current *username*

- **`~`**
Swicthes to the *home* directory

- **`mkdir`**`dir_name`
Creates a directory in the currect folder

	* **`-v`**
	*flag* that turns on *"verbose"* mode 

- `some_command`**`--help`**
Outputs a help menu of available commands along with their explanations
Must come *after* a valid command

- **`ls`**
List all of the files in the folder or directory

	* **`-l`**
	Formats the above command into a *table*

- **`rmdir`**`dir_name`
*Removes* a directory

### Working with Files

- **`touch`**`file_name.ext`
*Creates* a file in the current directory

- **`echo`**`"Some text"`**`>`**`file_name.ext`
*Overwrites* the quoted text into the file
Note: Using **`>>`** *appends* to an existing file
	
- **`nano`**`file_name.ext`
*Edits* the file in a text editor

- **`stat`**`file_name.ext`
Displays file stats and *octal* permissions

- **`chmod`**`0--- file_name.ext`
Modifies file *permissions*. Each `-` is an *octal* number between `0-7` corresponding to a combination of *read* **`r`**, *write* **`w`** and *execute* **`x`** for each scope (owner, group, everyone).

- **`mv`**`file_name.ext dir_name`
*Move* the specified file into the specified directory
Note: multiple files can also be specified between the command and destination

- **`cp`**`file_name01.ext file_name02.ext` 
Create a *copy* of a file within the current directory

- **`rm`**`file_name.ext`
Remove the specified file from current directory

### Working with Programs

- **`VAR`**`="This is text"`
We can create *Bash* variables and assign values to them
	* Note: retrieving value of the variable is done by `echo $VAR`

- **`export`**`VAR="This is text"`
Creates an *environment* variable
Accessing environment variables can also be done through Python.
```
import os
print(os.environ["VAR"])
```

## Intermediate

### Piping & Redirecting Output

- **`sort <`**`file_name.ext`
The above command will *sort* the lines in an alphabetical order
To *reverse*: `sort`**`-r`**`< filename.ext`

- **`grep`**`"string" file_name.ext`
Will find and print out all lines in the file containing *string*

- `grep "string" file_name`**`?`**`.ext`
Adding `?` uses a *wildcard* character to fit similar file names
An even broader *wildcard* character is **`*`**

- `python file_name.py`**`|`**`grep "some_string"`
*Piping* operator **`|`** will *chain* commands together to redirect standard output to standard input

- **`cat`**`filename.ext`
*Concatenates* the contents of the file 

- `echo "string" >> file_name.ext `**`&&`**` cat fil_ename.ext`
Chaining several commands with `&&` operator without passing ouput between them

- `echo "`**`\`**`"" >> file_name.ext`
Escape character **`\`** allows special characters to be treated as text

- **`head -n`**`10 file_name.ext`
List *first* 10 lines from the file

- **`tail -n`**`10 file_name.ext`
List *last* 10 lines from the file




		
