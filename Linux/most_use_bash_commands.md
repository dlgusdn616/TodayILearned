# Top 10 bash commands
#Linux/Bash


## 4) touch
Touches the file to update the access and or modification date of a file or directory without opening, saving or closing the file. But one of the most common uses is to create an empty file `touch my_file`.

## 5) cat
Concatenate and print files to stdout `cat my_file`. You can pass one or more file names to this command, and even number the lines using `-n` to number the lines. A close cousin of this is `vi` to launch a terminal-based text editor.

*For more on ways to use the cat command* - [13 Basic Cat Command Examples in Linux](https://www.tecmint.com/13-basic-cat-command-examples-in-linux/) 

Just see this linked note. [Cat Examples in Linux](bear://x-callback-url/open-note?id=FCA08CAD-F803-4411-BB04-9D677EFE6B93-320-00008FF1377E59CB)

## 6) mv
Moves files and folders. The first argument is the file you want to move, and the second is the location to move it to. Use the flags `-f` to force move them and `-i` to prompt confirmation before overwriting files.

Alternatively, you may not want to remove the original, for example when making backups, in this case you would use...

## 7) cp
Copies files and folders `cp my_file ./projects`. The flag `-r` recursively copies subfolders and files.

Now you have too many files, or some of them are no longer needed. The next command is a danger zone, because it is so good at its job...

# Danger Zone
- - - -
## 8) rm
Removes files and folders `rm my-folder`. Using `-r` will again recursively delete subfolders, `-f` force deletes, and `-rf` for a recursive force delete. If you want to remove all folders and files in the current directory the command is `rm -rf ./*`, if you leave out the dot then it would reference the root directory!

*Want to see what happens if you force a computer to run* [rm -rf /](https://www.youtube.com/watch?v=BhbLx0limX8)? *Tip: don't try this on yours, instead watch above video:*

There are ways to stop accidental deletes. If you use the `-i` flag (for interactive) you can specify the computer to prompt confirmation before delete.

## 9) chmod
Change mode so you can get permissions for read, write and execute for the user, members of your group and others. This uses binary values as an argument to set these. There are many common chmod permissions, a few key ones are:
* 777 - anyone can read, write, and execute `chmod 777 my_file`
* 755 - for files that should be readable and executable by others, but only changeable by the issuing user
* 700 - only the user can do anything to the file
* -rwxr-xr-x: a regular file whose user class has full permissions and whose group and others classes have only the read and executable permissions.
* `crw-rw-r--`: a character special file whose user and group classes have the read and write permissions and whose others class has only the read permission.
* `dr-x------`: a directory whose user class has read and execute permissions and whose group and others classes have no permissions.

![](most_use_bash_commands/35511FAC-DA4E-450F-84FC-CE9BA2924F27.png)

## 10) man
Manuals for a command can be shown with this instruction. Below is some of the output from running `man ls`, it also displays all the options available for running the command.
```
LS(1)                     BSD General Commands Manual                    LS(1)
NAME
     ls -- list directory contents
SYNOPSIS
     ls [-ABCFGHLOPRSTUW@abcdefghiklmnopqrstuwx1] [file ...]
DESCRIPTION
     For each operand that names a file of a type other than directory, ls displays its name as well as
     any requested, associated information.  For each operand that names a file of type directory, ls
     displays the names of files contained within that directory, as well as any requested, associated
     information.
```


