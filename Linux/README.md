# INDEX

- [INDEX](#index)
- [Introduction to Shell Basics](#introduction-to-shell-basics)
  - [What is Bash?](#what-is-bash)
  - [Fundamentals](#fundamentals)
  - [Shell Prompt](#shell-prompt)
- [File System in Linux](#file-system-in-linux)
  - [**Home directory** in Linux](#home-directory-in-linux)
  - [File Names](#file-names)
  - [Glob patterns](#glob-patterns)
  - [Exception patterns](#exception-patterns)
- [Bash Scripts](#bash-scripts)
  - [Defining variables](#defining-variables)
  - [Separating Commands with Semicolons](#separating-commands-with-semicolons)
  - [`#` and `@` expressions in a Bash Script](#-and--expressions-in-a-bash-script)
  - [Single Quotes vs. Double Quotes in Bash](#single-quotes-vs-double-quotes-in-bash)
  - [Invoking interpreters using shebang (`#!`)](#invoking-interpreters-using-shebang-)
- [Basic Command Reference](#basic-command-reference)
  - [`cd`](#cd)
    - [**Shortcuts**](#shortcuts)
  - [`ls` (TODO)](#ls-todo)
    - [Flags (Do in-depth)](#flags-do-in-depth)
  - [`pwd`](#pwd)
  - [`mv`](#mv)
  - [`touch`](#touch)
  - [`echo`](#echo)
  - [`env`](#env)
  - [`tee`](#tee)
  - [`dirname`](#dirname)
  - [`grep` (TODO)](#grep-todo)
    - [`dd` command (For making bootable USBs and swap partitions)](#dd-command-for-making-bootable-usbs-and-swap-partitions)
  - [`readlink`](#readlink)
    - [**flags**](#flags)
  - [command substitution (`$(...)`)](#command-substitution-)
  - [`chmod`](#chmod)
    - [Classes of users](#classes-of-users)
    - [Classes of permissions](#classes-of-permissions)
    - [Operators for changing permissions](#operators-for-changing-permissions)
  - [comment operator (`<<`)](#comment-operator-)
- [Subshells in Linux](#subshells-in-linux)
  - [How to create a subshell](#how-to-create-a-subshell)
  - [Subshell Applications](#subshell-applications)
    - [Using a subshell to set a temporary variable](#using-a-subshell-to-set-a-temporary-variable)
    - [Change directories without affecting the parent shell’s current directory](#change-directories-without-affecting-the-parent-shells-current-directory)
  - [Running commands in the background using Subshells](#running-commands-in-the-background-using-subshells)
    - [1. Basic Background Execution](#1-basic-background-execution)
    - [2. Running a Subshell in the Background](#2-running-a-subshell-in-the-background)
    - [3. Backgrounding an Already Running Process](#3-backgrounding-an-already-running-process)
    - [4. Checking Background Jobs](#4-checking-background-jobs)
    - [5. Bringing a Background Job to the Foreground](#5-bringing-a-background-job-to-the-foreground)
- [Package Management](#package-management)
  - [Advanced Packaging Tool (`apt`)](#advanced-packaging-tool-apt)
    - [Functions of `apt` tool](#functions-of-apt-tool)
  - [Tarballs](#tarballs)
    - [What is a tarball?](#what-is-a-tarball)
    - [Use of the `tar` utility](#use-of-the-tar-utility)
    - [Nested tarball extraction](#nested-tarball-extraction)
    - [Fun fact about tarballs](#fun-fact-about-tarballs)
  - [Debian Package Installer `dpkg`](#debian-package-installer-dpkg)
  - [Alien Package Converter `alien`](#alien-package-converter-alien)
- [Deepdives of Specific Commands](#deepdives-of-specific-commands)
  - [sh -c "$(wget \<URL\> -O -)"](#sh--c-wget-url--o--)
- [Some insights about Linux](#some-insights-about-linux)
  - [Why do Linux and Windows show different time in a Dual Boot?](#why-do-linux-and-windows-show-different-time-in-a-dual-boot)
  - [Terminal](#terminal)
    - [Terminals in earlier days](#terminals-in-earlier-days)
    - [Processing power of Terminals](#processing-power-of-terminals)
    - [Terminal Emulators](#terminal-emulators)
  - [Duplicate executables in folders that have been added to PATH](#duplicate-executables-in-folders-that-have-been-added-to-path)



# Introduction to Shell Basics

## What is Bash?

The Linux command line is provided by a program called the shell. The default shell for many Linux distros is the GNU Bourne-Again Shell (bash).

Bash is very powerful as it can simplify certain operations that are hard to accomplish efficiently with a GUI. Remember that most servers do not have a GUI.

## Fundamentals

- Unix-like operating systems and Windows have hierarchial directory structure (tree-like pattern of directories, called folders in other systems). 
- However, Windows has different drive letters for different storage devices and partitions, whereas Linux has only one file tree, and different devices can be on different branches of that tree.
- Linux has a root directory `‘/’` and all files and folders are contained inside it.
- The working directory on startup is `/home/<username>` which is the [**Home directory**](#home-directory-in-linux), but it can be anything set by the system administrator.

## Shell Prompt

When a shell is used interactively, it displays a `$` when it is waiting for a command from the user. This is called the **shell prompt**.

```console
rohan@ubuntu:~$ 
```

If shell is running as root, the prompt is changed to `#`. The superuser shell prompt looks like this:


```console
rohan@ubuntu:~#
```
Superuser has administrative privileges. This is dangerous, since superuser can delete/overwrite any file on the system. Operate as superuser ONLY when administrative 
privileges are needed.

# File System in Linux

## **Home directory** in Linux

`~` represents the **Home directory** of our linux system. Note that this is different from the `home` directory of the system. 

The **Home directory** stands for `/home/<user-name>` so we can use the tilde symbol (`~`) to not have to change our script for different users.

## File Names

- File names that begin with a period character are hidden. This means that `ls` will not list them unless we say `ls -a`, where `-a` means display all.
- File names in Linux, like Unix, are case sensitive.


## Glob patterns

Glob patterns, also known as wildcards, are a way of specifying file and directory patterns using a simple syntax. The syntax uses special characters to match one or more characters in file or directory names.

For example, the `*` character is a wildcard that matches any sequence of characters in a file or directory name. So, the pattern `*.txt` matches all files in the current directory that have a ".txt" file extension.

Here are some common glob patterns and their meanings:

- `*` : matches any sequence of characters in a filename
- `?` : matches any single character in a filename
- `[abc]` : matches any one character in the set of characters a, b, or c
- `[a-z]` : matches any one character in the range of characters from a to z
- `[^abc]` : matches any one character that is not in the set of characters a, b, or c

Glob patterns are commonly used in Unix-based operating systems for file manipulation, including finding, copying, and deleting files based on patterns. They can also be used in programming languages such as Python, Ruby, and JavaScript for pattern matching and filtering.

## Exception patterns

In the context of file or directory patterns, exception patterns are special patterns that exclude files or directories from a pattern match.

Exception patterns are used to specify files or directories that should not be matched by a [glob pattern](#glob-patterns). 

They are often used in conjunction with other **glob patterns** to create more complex pattern matching rules.

One common exception pattern is the `!` character, which specifies files or directories that should be excluded from a pattern match. 

For example, the pattern `*.txt` matches all files with a `.txt` extension, but the pattern `!README.txt` excludes the `README.txt` file from the match.

Here's an example that demonstrates the use of exception patterns:

```bash
# Match all files in the current directory except for README.txt
$ ls !(README.txt)

# Match all files in the current directory and its subdirectories except for *.log files
$ ls **/!(*.log)
```

> In the first example, the glob pattern `!(README.txt)` matches all files in the current directory except for `README.txt`. 
> 
> In the second example, the glob pattern `**/!(*.log)` matches all files in the current directory and its subdirectories except for files with the `.log` extension.

Exception patterns can be very useful when you need to exclude certain files or directories from a pattern match. They provide a flexible way to specify complex pattern matching rules in a single glob pattern.

# Bash Scripts

Scripts start with a bash bang.
Scripts are also identified with a shebang. Shebang is a combination of `bash #` and `bang !`  followed by the bash shell path. 

```bash
#! /bin/bash
```

This is the first line of the script. Shebang tells the shell to execute it via bash shell. Shebang is simply an absolute path to the bash interpreter.

## Defining variables

We can define a variable by using the syntax `variable_name=value`. 

To get the value of the variable, add `$` before the variable name.

```bash
#!/bin/bash
# A simple variable example

greeting=Hello

echo $greeting
```

This can be considered as an example of [command substitution](#command-substitution) as we get values of variables when we type variable names directly in the terminal. 

## Separating Commands with Semicolons

When the shell sees a semicolon on a command line, it's treated as a command separator -- basically like pressing the ENTER key to execute a command. 

When would you want to use a semicolon instead of pressing ENTER? 
-  It's nice when you want to execute a series of commands, typing them all at once at a single prompt. 
    All of them will be visible on the same command line and they'll be grouped together in the history list.
  
    This makes it easy to see, later, that you intended this series of commands to be executed one after another. And you can re-execute them all with a simple history command. 

- It's useful with sleep to run a command after a delay. 
  ```bash
  echo "Type CTRL-c to abort logout"; sleep 10; exit
  ```
-  If you're running a series of commands that take some time to complete, you can type all the commands at once and leave them to run unattended. For example, if a two scripts need to be run in succession and they take a lot of time this is helpful.

## `#` and `@` expressions in a Bash Script

`@` - The array of parameters passed to a bash script
`#` - The number of positional parameters passed to a script.

We can use the command substitution method `$@`/`$#` in order to get the values of these expressions.

## Single Quotes vs. Double Quotes in Bash

Comprehensive documentation:
- [Single Quotes](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html#Single-Quotes)
- [Double Quotes](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html#Double-Quotes)

1. For command or parameter substitution, use double-quotes, since no character has special meaning within single quotes. Example:
   
  ```sh
  echo "$ENV_VAR"
  # OR
  bash -c "$( wget URL )"
  ```

  Using double-quotes helps retain the meaning of `$` for parameter substitution and `$(...)` for command substitution.

2. If quotes within quotes need to be used:
   A. Double quotes within single quotes
    ```sh
    bash -c 'echo "hello"'
    ```

   B. Double quotes within single quotes (the quotes inside should be preceded by backslashes)
    ```sh
    bash -c "echo \"hello\""
    ```

## Invoking interpreters using shebang (`#!`)

Shebang is a character sequence consisting of a hash sign and an exclamation mark (`#!`) and is used to tell the operating system which interpreter to use to parse the rest of the file.

The Shebang interpreter directive takes the following form:
```bash
#!interpreter [arguments]
```

The directive must be the first line in the script.

The directive must start with shebang (`#!`).

White space after the shebang characters (`#!`) is optional.

Interpreter is the full path to a binary file (ex: `/bin/sh`, `/bin/bash`).

Interpreter arguments are optional.

For example:

- `#!/bin/bash` - Uses bash to parse the file.
- `#!/usr/bin/env perl` - Uses the [`env`](#env) command to find the path to the perl executable.
- `#!/usr/bin/python` - Executes the file using the python binary.

Take a look at this example, where we invoke the `python3` interpreter, and run python code within a `.sh` script.

```python
#!/usr/bin/python3

print("Hello world")

n = 0

for i in range(0, 5):
    if(n % 2 == 0):
        print("Even")
    else:
        print("Odd")
    n += 1
```


# Basic Command Reference

## `cd`

Command for changing working directory.

Syntax: `cd <path name>`  
`<path name>` can be **absolute** or **relative** to the current working directory.

- **Absolute Pathname** starts with the root directory and follows the tree branch by branch until the path to the desired directory or file is completed. Example:  
  `/usr/bin` means
  - `/`: root directory
  - `user`: subdirectory of root
  - `bin`: subdirectory of `user`
- **Relative Pathname** starts from the working directory. Special notations:
  - `.` refers to the working directory itself. Example:
  ```console
  root@ubuntu:~$ cd ./Downloads
  root@ubuntu:~/Downloads$
  ```
  where `~/Downloads` stands for `/home/<username>/Downloads`.

  - `..` refers to the parent directory of the working directory. Example:
  ```console
  root@ubuntu:/usr/bin$ cd ..
  root@ubuntu:/usr$
  ```
  In both the above cases, we can make this change using absolute pathnames as well.

### **Shortcuts**

- `cd`: changes working directory to home directory
- `cd -`: changes working directory to the previous one
- `cd ~userName`: changes working directory to the home directory of the specified user  
<br>

## `ls` (TODO)

Command for listing files and directories.

### Flags (Do in-depth)

- `-R`: Recursively searches inside all folders in a directory.
- `-h`: Displays file sizes in human-readable format
- `-l`:

## `pwd`

Command for printing working directory.

## `mv`

To move a file in a terminal, we use the `mv` command to move a file from one location to another.

Note that if in case the destination has a file with the same name, it is OVERWRITTEN.

```console
root@ubuntu:~/Downloads$ mv example.txt ~/Documents

root@ubuntu:~/Downloads$ ls ~/Documents
example.txt
```
The file `example.txt` was moved from `/home/<username>/Downloads` to `/home/<username>/Documents`.

An important thing to note is, remember to use `sudo` when performing operations like moving because in most systems, all users don't have WRITE access everywhere. 

An alernative to this would be to use the command `chmod a+w ./path` to give everyone WRITE access at the place we wish to move the file but this could lead to vulnerabilities.

## `touch`

Syntax: `touch <filename>`
 
Navigate to the folder where the file is to be created or open a terminal within that folder. Run a command in the format shown above.

For example:
```console
root@ubuntu:~$ touch test.txt
root@ubuntu:~$
```
where the text between the `:` and the `$` sign stands for the current working directory (`~` is the Home directory).

## `echo`

Syntax: `echo <string>`  
Used to display `<string>` on the command-line. It is a built-in command used in shell scripts and batch files to output status text to the screen or a file.

## `env`

`env` is a shell command for Unix and Unix-like operating systems. 

- It is can be used to print a list of environment variables like this:

  ```
  SHELL=/bin/bash
  QT_ACCESSIBILITY=1
  COLORTERM=truecolor
  XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu:/etc/xdg
  GNOME_DESKTOP_SESSION_ID=this-is-deprecated
  LANGUAGE=en_IN:en
  GNOME_SHELL_SESSION_MODE=ubuntu
  local/share:/usr/share:/var/lib/snapd/desktop
  PATH=/usr/local/bin:/usr/sbin:/usr/local/games:/snap/bin
  GDMSESSION=ubuntu
  ...
  ...
  ...
  ```

  Using `env`, variables may be added or removed, and existing variables may be changed by assigning new values to them.

- In practice, `env` has another common use as a utility. It is often used by shell scripts to [launch the correct interpreter](#invoking-interpreters-using-shebang).
  ```
  #!/usr/bin/env bash
  ```
  The advantage of using this approach is that it will search for the `bash` executable in the user’s `$PATH` environmental variable. 

  If there are more than one paths to `bash`, the first one will be used by the script.

## `tee`

The tee command reads standard input (stdin) and writes it to both standard output (stdout) and one or more files. tee is usually part of a pipeline, and any number of commands can precede or follow it.

So, for example, if we wish to append a new command to an executable file :
```console
root@ubuntu:~$ echo '<command>' | tee -a filename.sh
<command>
rohan@rohan-G3:~$ 
```
- The output of `echo '<command>'`, which is `<command>`, is displayed on the standard output device as shown, along with `<command>` being written to the file `filename.sh`.

- Here, the `-a` flag represents that the file should not be overwritten and the command should be appended to its end.

We can also prefix `sudo` to the `tee` command to be able to write to files that are read-only for guest users.
```bash
echo '<command>' | sudo tee -a filename.sh
```

Refer [this](https://phoenixnap.com/kb/linux-tee#:~:text=What%20Does%20tee%20Command%20Do,can%20precede%20or%20follow%20it.) for more information about `tee`.

## `dirname`

Syntax: `dirname <file-path>`  

This command prints the directory containing the supplied path.
If we supply a file/directory, `dirname` outputs the path containing that file/directory. 

For example:
```console
root@ubuntu:~$ dirname /home/example/foo
/home/example
root@ubuntu:~$
```

## `grep` (TODO)

### `dd` command (For making bootable USBs and swap partitions)

Refer [this](https://linuxhint.com/dd_command_linux/).

## `readlink`

Used to obtain the full path of a file. `readlink` prints the absolute path of a symbolic link (a type of file in Linux that points to another file or a folder on your computer. Symlinks are similar to shortcuts in Windows.) or the absolute path for a supplied relative path.  
NOTE: It is vital that our current directory is a parent directory or higher of the file in question.

Syntax: `readlink [<flag>] <relative-path/file-name>`

### **flags**

If the conditions specified by these flags aren't met, an output isn't returned.
- `-f` : All but the last component (which is the file itself) in the supplied path must exist. If the path does not exist.
- `-e` : All components in the supplied path must exist.
- `-m` : None of the components in the supplied path are required to exist.

For example, obtaining absolute path using the relative path:
```console
root@ubuntu:~/home/example$ readlink -f foo/foo.txt
home/example/foo/foo.txt
root@ubuntu:~/home/example$
```

## command substitution (`$(...)`)

Syntax: `$(command)`  
where `command` is executed in a sub-shell, and the output from `command` replaces its call.<br><br>
Command substitution can be used in combination with `dirname` to obtain the name of the directory which contains a specific file, without knowing even the relative path of that file.
```console
root@ubuntu:~$ dirname $(readlink -f file.txt)
/home/example/foo
root@ubuntu:~$
```

## `chmod`

`chmod` allows control of read, edit and run permission for files and directories. `chmod` stands for change 
mode.   

### Classes of users

- user `u` : the person who created the file  
- group `g`: people in a selected group  
- other `o`: everyone else on the system  

### Classes of permissions  

- read `r`   : ability to see the contents of the file  
- write `w`  : ability to change the contents of the file  
- execute `x`: ability to execute the contents of the file  

### Operators for changing permissions

- plus `+` : the permission is added.
- hyphen  `-` : the permission is disabled.
- equal-to `=` : the permission is explicitly assigned. All other settings for that category (owner, group, or others) are reset. For example, `g=r` removes all permission from the group except read, and explicitly set read if not set already.

Syntax: `chmod u+x <file-name>`  

This command is used to give the shell script `<file-name>` executable permissions. 

A file retains the permissions it had even when its name is changed.

## comment operator (`<<`)
Syntax:
```console
<<comment-name
//Comment body

comment-name
```
where `comment-name` can be replaced by any string.

---

# Subshells in Linux

A subshell is a child shell that is spawned by the main shell (also known as the parent shell). 

It is a separate process with its own set of variables and command history, and it allows you to execute commands and perform operations within a separate environment.

## How to create a subshell 

- Use the parentheses operator, like this:
  ```sh
  ( command1 ; command2 ; command3 )
  ```
  OR
  ```sh
  ( 
    command1 
    command2
    command3 
  )
  ```
  
  This will execute the commands within the parentheses in a subshell. 
  
  The commands are separated by semicolons, and the subshell will terminate when all commands have completed execution.

- Use the `bash` OR `sh` command, like this:
  ```sh
  bash -c "command1 ; command2 ; command3"
  # OR
  sh -c "command1 ; command2 ; command3"
  ```

  This will execute the commands within the quotation marks in a subshell created by the bash command.

## Subshell Applications

Subshells allow you to isolate the effects of certain commands or operations. 

For example, you can use a subshell to set temporary variables or change directories without affecting the parent shell’s environment.

> ***Note***: Command substitution actually makes use of subshells to execute commands and then the output is substituted in place of the dollar.

### Using a subshell to set a temporary variable

```sh
# Set a variable in the parent shell
FOO=bar

# Create a subshell and set a different value for the same variable
( 
  FOO=baz
  echo $FOO 
)

# The value of FOO in the parent shell is unchanged
echo $FOO
```
 
Output:
```
baz
bar
```
 
As you can see, the value of the FOO variable is set to baz within the subshell, but the value in the parent shell remains unchanged.

### Change directories without affecting the parent shell’s current directory

```sh
# Show the current directory in the parent shell
pwd

# Create a subshell and change to a different directory
( cd /tmp ; pwd )

# The current directory in the parent shell is unchanged
pwd
```

Output:
```
/home/user
/tmp
/home/user
```

As you can see, the subshell changes to the `/tmp` directory, but the parent shell’s current directory remains unchanged.

---

## Running commands in the background using Subshells

Subshells can be used to execute commands in the background by appending an ampersand (`&`) to the end of the command. 

When you do this, the command will run in a subshell as a background process, allowing you to continue using the terminal while the command executes independently.

When running commands in the background, they will continue to execute even if you log out of your terminal session. To manage background processes more effectively, tools like `screen` or `tmux` can be used to create persistent terminal sessions that can be detached and reattached.

### 1. Basic Background Execution

```bash
( command-to-run & ) 
```

For example:

```bash
( sleep 10 & )
```

In this example, the `sleep 10` command is executed in the background, and the terminal prompt returns immediately. You can continue working in the terminal while the `sleep` command runs in the background for 10 seconds.

### 2. Running a Subshell in the Background

You can also run an entire subshell in the background, which can be useful for running multiple commands concurrently:

```bash
( commands-to-run-in-subshell ) &
```

For example:

```bash
( echo "Task 1" ; sleep 5 ; echo "Task 2" ) &
```

This will run a subshell with three commands (`echo "Task 1"`, `sleep 5`, and `echo "Task 2"`) in the background.

### 3. Backgrounding an Already Running Process

If you have a command that is already running in the foreground, you can suspend it (usually by pressing `Ctrl+Z`) and then use the `bg` command to send it to the background:

```bash
Ctrl+Z
bg
```

This will resume the suspended command in the background.

### 4. Checking Background Jobs

You can check the status of background jobs using the `jobs` command:

```bash
jobs
```

This will list the background jobs along with their job numbers.

### 5. Bringing a Background Job to the Foreground

If you want to bring a background job back to the foreground, you can use the `fg` command followed by the job number:

```bash
fg %1
```

In this example, `%1` refers to job number 1 as shown by the `jobs` command.

---

# Package Management

**Debian** has a robust packaging system and every component and application is built into a package that is installed on your system. 

## Advanced Packaging Tool (`apt`)

Debian uses a set of tools called **Advanced Packaging Tool** _(APT, not to be confused with the command `apt`)_ to manage this packaging system.

There are various tools that interact with APT and allow us to install, remove and manage packages in Debian based Linux distributions. `apt-get` is one such command-line tool which is widely popular.
However there is a problem of redundancy when it comes to `apt-get` commands, as there are a number of similar commands in the `apt-cache` tool.

Also, these commands are way too low level and they have so many functionalities which are perhaps never used by an average Linux user.
On the other hand, the most commonly used package management commands are scattered across `apt-get` and `apt-cache`.

The `apt` tool was introduced to solve this problem. `apt` consists some of the most widely used features from `apt-get` and `apt-cache` leaving aside obscure and seldom used features.

### Functions of `apt` tool

- Maintains a package list, meaning it fetches a list of all the available software that can be installed on the platform. 
- `sudo apt update`: Updates the package list
- `sudo apt upgrade`: Upgrades the previously installed software to newer versions
- `sudo apt install <package-name>`: Used to install new software packages
- `sudo apt remove <package-name>`: Removes pieces of software WITHOUT breaking the system

## Tarballs 

### What is a tarball?

In Linux, a `"tarball"` is a type of archive file format used for bundling multiple files and directories into a single file for easy storage, transfer, and distribution. 

Tarballs are commonly used in Linux for packaging and distributing software applications, libraries, and other collections of files. 

They can be easily extracted using the `tar` command, which can *recursively extract* all files and directories within the archive to their original locations. This makes it a convenient way to transport and install software packages on Linux systems.

### Use of the `tar` utility

A tarball is created using the `tar` command and typically has a `.tar` extension at the end of the filename. 

However, it can also be compressed using other tools such as `gzip` or `bzip2`, resulting in file extensions such as `.tar.gz` or `.tar.bz2`. This compression reduces the overall size of the archive, making it easier to transfer over networks or store on limited storage space.

### Nested tarball extraction

Tarballs within tarballs can be extracted using the `tar` command. This is known as **"nested"** or **"embedded"** archives.

When a tarball contains another tarball, it is called a "tarball within a tarball" or a "nested tarball". 

Process of nested extraction:

1. To extract the files from a nested tarball, you can use the tar command with the `--extract` or `-x` option followed by the name of the outer tarball file. The -v (verbose) option can also be used to display the progress of the extraction.

2. The contents of the nested tarball file will be extracted to a directory within the extraction directory of the outer tarball file. 
   
   For example, suppose you have a tarball `outer.tar` that contains a nested tarball `inner.tar`. If you extract `outer.tar`, the extracted files from `inner.tar` will be located in a directory called `inner`, which will be located in the same directory as the extracted files from `outer.tar`.

It is important to note that if the nested tarball file is compressed, such as with `gzip` or `bzip2`, you will need to use the appropriate decompression tool, such as `gzip` or `bzip2`, before extracting it with the `tar` command.

> ***Note***: This is the default behavior of the `tar` utility. If you only want one level of tarball extraction, use the `--no-recursion` flag.

### Fun fact about tarballs

The term `"tarball"` is derived from the use of the `tar` command, which is used to create and manipulate these files.

Most people would think it was the other way around.

## Debian Package Installer `dpkg`

`dpkg` is a tool to install, build, remove and manage Debian packages. The primary and more user-friendly front-end for `dpkg` is aptitude(1). 

Syntax:
```
dpkg [option...] action
```

`dpkg` itself is controlled entirely via command line parameters, which consist of exactly one action and zero or more options. 

The action-parameter tells `dpkg` what to do and options control the behavior of the action in some way.

Refer the official documentation on [man7.org](https://man7.org/linux/man-pages/man1/dpkg.1.html) to find out how to use `dpkg` and its various actions.

The simplest way to use `dpkg` to install a `.deb` package.
```
dpkg -i <package>.deb
```

## Alien Package Converter `alien`

Alien is a tool that converts different Linux package distribution file formats to Debian. 

It supports conversion between Linux Standard Base, `.rpm`, `.deb`, Stampede (`.slp`) and Slackware (`.tgz`) packages.

Ubuntu is a popular Debian based Linux distribution, and as such it uses `.deb` packages for installing software.

However, not all software is packaged in the `.deb` format. Some software maintainers only provide a `.rpm` package, which is a format used exclusively with Red Hat and CentOS based distributions.

Refer [this](https://www.serverlab.ca/tutorials/linux/administration-linux/how-install-rpm-packages-on-ubuntu-using-alien/) tutorial for information on how to use it.

---

# Deepdives of Specific Commands

## sh -c "$(wget \<URL\> -O -)"

This approach is commonly used for downloading and executing scripts directly from the web. 

It allows users to install software or configure their systems with a single command, pulling the necessary scripts or files from the internet.

Here's a breakdown of how the command works:

1. `sh -c`: This part of the command invokes a subshell (`sh`) and uses the `-c` option to specify that the following string should be treated as a command and executed by the subshell.

2. `$(...)`: This is a command substitution. The expression inside the `$()` is executed, and its output is used as part of the command. In this case, it's used to execute the `wget` command and fetch the script from the specified URL.

3. `wget \<URL\> -O -`: This part of the command uses the `wget` utility to download a script from the specified URL.
   
    `-O` specifies the output file, and `-` as its argument means that the output should be directed to the standard output (stdout) instead of saving it to a file.

So, when you run the entire command, here's what happens:

1. A subshell (`sh`) is invoked with the `-c` option.
2. Inside the subshell, the `wget` command is executed to download the installation script.
3. The downloaded script is passed as input to the subshell for execution.


--- 

# Some insights about Linux

## Why do Linux and Windows show different time in a Dual Boot?

A computer has two main clocks: a **system** clock and a **hardware** clock.

A hardware clock which is also called RTC (real time clock) or CMOS/BIOS clock. This clock is outside the operating system, on your computer’s motherboard. It keeps on running even after your system is powered off.

The system clock is what you see inside your operating system.

When your computer is powered on, the hardware clock is read and used to set the system clock. Afterwards, the system clock is used for tracking time. If your operating system makes any changes to system clock, like changing time zone etc, it tries to sync this information to the hardware clock.

By default, Linux assumes that the time stored in the hardware clock is in UTC, not the local time. On the other hand, Windows thinks that the time stored on the hardware clock is local time. That’s where the trouble starts.

Refer [this](./linux-windows-time-diff.sh) executable shell script for resolving this issue.

## Terminal

### Terminals in earlier days

In the early days of computing, a `terminal` referred to a physical device that provided a user interface for interacting with a remote computer system. 

These devices were typically connected to a mainframe or minicomputer using a serial or parallel connection, and provided a way for users to enter commands and receive output.

The earliest terminals were indeed monochrome devices that displayed only text, typically using a **fixed-width** font. These terminals were often referred to as "dumb terminals", because they had no processing power of their own and simply transmitted data back and forth between the user and the remote computer.

### Processing power of Terminals

Terminals did require some amount of processing power to display characters being typed.
 
They had some level of processing capability in order to buffer incoming data and display it on the screen in real-time.

In early terminals, this processing power was provided by dedicated hardware components such as the terminal controller, which managed the communication between the terminal and the mainframe, and the display controller, which generated the video output to the screen. 
 
These components were usually integrated into the terminal itself.

Later, as terminals became more sophisticated, they included additional hardware such as microprocessors, memory, and storage, which allowed for more advanced features such as cursor control, scrolling, and color display.  
 
However, the processing power of these terminals was still orders of magnitude less than what we have in modern personal computers.

### Terminal Emulators

Over time, the capabilities of terminals expanded to include support for color, graphics, and other advanced features. 

With the advent of personal computers and graphical user interfaces, the use of traditional terminals declined in favor of GUI-based systems.

However, the concept of a `terminal` is still relevant today, with modern **terminal emulators** providing a way to interact with command line interfaces on remote systems from a local computer.


## Duplicate executables in folders that have been added to PATH

When you try to execute a binary that is present in two folders that have been added to the `PATH` environment variable, the operating system will search for the binary in the directories specified in the `PATH` variable in order from left to right until it finds a matching binary.

For example, suppose you have added the directories `/usr/bin` and `/usr/local/bin` to your `PATH` variable, and both of these directories contain a binary called `example`. 

If you type `example` into the command prompt, the operating system will search for the binary in the directories specified in the `PATH` variable in the order `/usr/bin` first and then `/usr/local/bin`. 

If the binary is found in `/usr/bin`, that version of the binary will be executed. 

If it is not found there, the operating system will continue to search in the next directory specified in the `PATH` variable, in this case `/usr/local/bin`. 

If the binary is found in that directory, that version of the binary will be executed.

> ***Note***: The order of directories in the `PATH` variable is important, as it determines the order in which the operating system searches for binaries. 
> 
> If you want to ensure that a specific version of a binary is always executed, you can reorder the directories in the `PATH` variable accordingly.

