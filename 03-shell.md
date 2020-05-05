# Understanding a Linux based OS:  S1E3 - Shell
> Shell is a programa that interprets commands and executes the programs specified by them
- They are the gateway to the system
- `sh`, `bash` and `zsh` are some of the popular shells
- A command, with its arguments is called **command line**
```shell    
        echo  -e "Good Day!\n"
       \----|---------------/
          |          |
          V          V
       Command    Arguments
       \--------------------/
                 |
                 V
           Command Line
```
- Shell parses the command line, and executes the program using `fork()`
- During parsing, shell performs *expansion* of some of its arguments, which can be seen using **echo**
- We can get the previous commandline using `!!`
----
## Finding the executables - The `$PATH`
- Shell variables can be used to store values
- They begin with `$` when accessing. While setting, they are set by their name. They are case-sensitive.
```shell
        # A comment line begins with # 
        a=10 # Note that there are no spaces around '='
        echo $a # Accessing
        a= # Clearing the value
```
- Whenever a command is entered, it is searched in a list of some pre-defined directories. This `:` delimited list is stored in the variable `$PATH`
- `where` or `which` can be used to locate the directory in which a command is located
- But, some commands like `cd` and `kill` does not exist anywhere. They are **Shell built-in commands**
---------
## Environment variables
- Shell sets some variables while it is executing, which defines its behaviour.
- They can be viwed using `env` command
- Some important environment variables are described below

 Variable | Significance 
----------|-------------
`$PATH` | List of directories in which command is looked up
`$PWD` | Current working directory. Can also be got using `pwd`
`$USER` | Name of current user
`$HOME` | Home directory of current user. Also `~`
`$SHELL` | Location of the shell being used
`$EDITOR` | Default text editor
`$PROMPT` | Prompt of the shell. By default, `$` for normal user, and, `#` for super user (root)
`$TTY` | The `tty` or `pts` device on which the current shell is being active
-----
## Modifying environment variables
- We mostly modify `$PATH`, in order to add a directory to global list of paths from where the executables are searched
- `PATH=$PATH:<new dir>`, remeber to append to existing path, or else everyting will be lost! (for that session, though :P)
- `export PATH=$PATH:<new dir>` also works, this makes the path visible to (i.e., exports to) child programs of the shell
- But this change is temporary, thus, we have to do this everytime we want the change in effect
- There must be some way to execute some commands whenever the shell gets started
------
## The run command (`rc`) files
- Executed during initialization of the system, or, the shell
- `~/.bashrc` is the file which is executed whenever a new session of `bash` is created
- We can append the commands which are supposed to execute at the startup of terminal to `~/.bashrc`
- We can re-execute the `rc` file in the middle of shell session by using `source`.
```shell
        source ~/.bashrc
```
- There are other `rc` files for applications, and system startup
- [`oh-my-zsh`](https://github.com/ohmyzsh/ohmyzsh) is a framework for customizing `zsh` using such scripts
-----
## Shell scripts
- Commands, with control sequences to be executed as a script
- They are interpreted by the shell
- If there is an error at line `i`, the program execute till line `i`, and shows error there. This is because of the interpreted nature of the shell
- First line starting with `#!` tells which shell interpreter to use. Common interpreters are `/bin/bash`, `/bin/zsh`, `/bin/perl` and `/bin/python`. This line is popularly called *shebang* line.
- Some special variables provide information of context of current script being exected. Some are listed as follows.

Variable | Significance 
---------|--------------
`$#` | Number of arguments passed. (like `argc`)
`$n` | `n`<sup>th</sup> argument (like `argv[n]`)
`$0` | Name of the currently executing file (i.e., `argv[0]`)
`$@`, `$*` | All arguments passed (i.e., `argv[1:]`)
`$$` | Current PID

- There are loops, arithmetic operations, string operations, functions...
<!--show some examples!-->

-------------
## Executing Shell Scripts
- Any text file, by default, is not executable
- We have to set the executable flag using `chmod`
   - `chmod [-R] <permissions> <file>`
   - If `-R` is present, permissions are modified recursively
   - Permissions can be of 2 forms
        - In first form, target audience and permission are specified
        - There can be 3 type of audience

          Target Audience | Notation
          --------------|------------
          Current User | `u`
          Current User's Group | `g`
          Others | `o`

        - 2 types of mode

          Mode | Notation
          ----------|---------
          Grant     | `+`
          Revoke    | `-`

        - 3 permissions

          Permission | Notation
          -----------|-----------
          Read | `r`
          Write | `w`
          Execute | `x`
        - The final permission is `<target audience><mode><permissions>,...`. ex., `u+rwx,o-w,g+x`
  - Another method is to specify final permissions using octal numbers

        ---------------------
        |  u  ||  g  ||  o  |
        |-------------------|
        |r|w|x||r|w|x||r|w|x|
        ---------------------
  - The popular (and dangeours) example is `777`, which gives every permission to everyone.
- `chown` is a related command. It is used to set the owner of the file.
  - `chown [-R] user:group file`
- `chgrp` is also there, which just changes the group.
- Safest method to make script executable is `chmod +x script.sh`
- Then, it can be executed as `./script.sh`
- Since `.` was in `$PATH`, we had to use `./`
------------
## Command aliases
- Anologous to symlinks in filesystem
- Long to type, frequently used commands are alised to some short commands
- `alias` to view all aliases
- `alias short=long` to set alias of `long` as `short`
- Aliases are temporary, and live until the end of the session
- In order to make them permament, append aliasing command to end of `rc` file of your shell
- Popular alias is `alias ll=ls -l`
- Note that there is no space around `=`
-----------
## Text utilities
- Shell provides many utilities to manipulate text
- They can either take input from file, or through pipe (`|`)
- Following is the list of some popular text utilities, along with their uses

Utility | Uses 
--------|------
`echo`  | Print
`cat`   | Con**cat**enate :cat:
`grep`  | **G**lobal **R**egular **E**xpression **P**rint. prints occurances of the text matched by [RegEx](https://regexr.com/) 
`sed`   | **S**tream **Ed**itor. Popular for find and replace like operations based on RegExs on streams and files
`tr`    | **Tr**anslate. Translates a list of source characters to destination characters
`cut`   | Splits the stream based on given delimiter, and returns specified fields
`paste` | Joins the content of input files onto a line with a specified delimiter
`sort`  | Sorts the input stream
`uniq`  | Display unique entries
`less` and `more` | Pagers, divide the text crossing the window boundaries into pages
`wc` | **W**ord **c**ount. Gives character, spaces and newline count

We can combine them using pipes to achive variety of useful tasks.

---------------
## Shell redirections
- By default, each process reads from `stdin`, and writes to `stdout` and `stderr`
- We can instead hook them to other files
- [Following table](https://thoughtbot.com/blog/input-output-redirection-in-the-shell) shows the methods to do so

Symbol | Target
-------|--------
`>file`    | `stdout` is written onto `file`
`<file`    | `stdin` is read from `file`
`>>file`   | `stdout` is appended onto `file`
`x>&y` | File Descripter `x` is written onto File Descriptor `y`.`fd` of `stdin` is `0`, `stdout` is `1` and `stderr` is `2`
- `command >/dev/null 2&>1` executes the command silently, as it redirects `stdout` to `/dev/null` (remember that it is the infinite sink) and then `stderr`(`fd` `2`) to `stdout`(`fd` `1`)
- But this process is still hanging on terminal!
-------
## Background Processes
- Long running processes with no interaction requried from user hang a terminal sesssion with their output
- Although we can silent them (using redirection), they still hang the terminal and require us to open a session
- They can be created by appending a `&` to their commandline
        
        some-long-running-task &
- `jobs` is used to view jobs 
- `fg` is used to bring background jobs onto foreground
- `bg` is used to run suspended jobs (`Ctrl + Z`ed) jobs in the background
- `kill %i` is used to kill the job with job id `i`