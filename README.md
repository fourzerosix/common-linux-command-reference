# Linux Command Reference

## Summary
This document serves as a quick-reference guide for essential Linux command-line operations relevant to users working on cluster systems. It includes frequently used file system operations, process management, job scheduling (SLURM), SSH, and session management tools. This document is intended for our cluster users who need a quick lookup for common Linux tasks and SLURM job scheduling commands.

---

## Prerequisites
  * Linux command line is case sensitive
  * The dolloar sign (`$`) indicates the beginning of a command
  * The hash/pound sign (`#`) indicates end of a command the start of a comment
  * the notation `<some_variable>` refers to variables and file names that need to be specified by the user. The symbols `<` and `>` need to be excluded

---

## Command Reference
  
### Viewing and Changing Directories
```bash
$ pwd           # get the full path of the present working directory
$ ls            # list content of pwd
$ ls -l         # similar to ls, but provides more information on files and directories
$ ls -a         # includes hidden files (.name) as well
$ ls -R         # lists subdirectories recursively
$ ls -t         # lists files in chronological order
$ cd <dir>      # brings you to the indicated <dir>ectory
$ cd ~          # brings you to your home directory
$ cd ..         # moves you one directory up
$ cd ../..      # moves two directories up (and so on)
$ cd -          # go back to where you were before the last directory change
```

* *The tilde symbol (`~`) is the path to your home directory*
* *Period/dot (`.`) refers to the present working directory*

```bash
$ echo ~        # view the complete path of your home (similar to pwd)
$ find ~        # list all your files (including everything in sub-directories)
$ ls ~          # list the top level files and directories of your home directory
$ ls .          # list the files and directories in the directory you are currently in
$ du -sch ~/*   # calculate the file sizes in your home
```

---

### Viewing Information
```bash
$ stat <file>            # Last modification time stamps, permissions, and size of a <file>
$ hostname               # shows which machine you're on (same as "echo $HOSTNAME")
$ df -h                  # disk space
$ free -g                # memory info in Megabytes
$ uname -a               # shows info about system
$ ip a                   # give IP and other network info
$ du -sh                 # displays disk space usage of current directory
$ du -sh *               #displays disk space usage of individual files/directories
$ du -s * | sort -nr     # shows disk space used by different directories/files sorted by size
```

---

### Viewing Text
```bash
$ more <file>                 # views text, use space bar to browse and q to text
$ less <file>                 # more versatile text viewer than 'more'
                                # q to exit
                                # G to jump to end of text
                                # g to go to beginning
                                # / find forward
                                # ? find backwards
$ cat <file> <output>    # concatenates files and prints content to standard output
$ head -<number> <file>  # prints first lines of a <file>
$ tail -<number> <file>  # prints last lines of a <file>
```

---

### Files and Directories
```bash
$ mkdir <dir>                       # creates specified <dir>ectory
$ rmdir <dir>                       # removes an empty <dir>ectory
$ rm <file>                         # removes <file>
$ rm -r <dir>                       # removes <dir>ectory including its content, but asks for confirmation, 'f' argument turn confirmation off
$ cp <file> </path/to/target>       # copy file/directory as specified in path (-r to include content in directories)
$ mv <old_name> <new_name>          # renames directories or files
$ mv <file> </path/to/target>       # moves file/directory as specified in path
```

---

### File Manipulation
```bash
$ cat <file1> <file2> <output> # concatenate files 1 and 2 into <output> file
```

---

### Handy Shortcuts
* Using the up arrow and down arrow will scroll through command history
* History shows all commands you have used recently
* The tab key will auto complete a file/directory/path
* Execute two commands together/consecutively with a pipe (`|`) - `;` - `&&`
* Control shortcuts in command line
  `ctrl+a` # cursor to beginning of command line  
  `ctrl+e` # cursor to end of command line  
  `ctrl+w` # cut last word  
  `ctrl+k` # cut from cursor to end of the line  
  `ctrl+y` # paste content that was cut by `ctrl+w` or `ctrl+k`  

### Unix Help
```bash
$ man <command>      # manual page/general help for most linux commands
$ man wc             # manual on program 'word count' wc
$ wc --help          # short help on wc
$ soap -h            # for less standard programs
```

---

### Finding Things
```bash
$ find </path/to/location> -options <what_to_find> 
$ find . -name *.txt           # find files in the current directory that end in .txt
$ find . -iname *.txt          # same as above, but case insensitive
$ find ~ -type f -mtime -2     # find all files you have modified in the last two days
$ locate <pattern>             # finds files and directories that are written into update
$ which <application>          # find location of <application>
$ whereis <application>        # searches for executables in set of directories
$ rpm -qa | grep <application>   # searches for installed .rpm files
```
**`grep`**
```bash
$ grep <PATTERN> <FILE_NAME>       # provides lines in file where pattern appears
$ grep -H <PATTERN>                # -H prints out file name in front of pattern 
$ grep <'PATTERN'> <FILE_NAME>     # if pattern is shell function uses single quotes
$ grep <PATTERN> <FILE_NAME> | wc  # pipes line with pattern into word count
```
**`wc`**
```bash
$ wc <FILE_NAME>      # output shows number of lines, words, bytes
$ wc -l <FILE_NAME>   # print number of lines in a file
$ wc -w <FILE_NAME>   # print number of words in a file
$ wc -c <FILE_NAME>   # display the count of bytes in a file
$ wc -m <FILE_NAME>   # print the count of characters from a file
$ wc -L <FILE_NAME>   # print only the length of the longest line in a file
```
**`sed`**
```bash
$ sed 's/<ORIGINAL>/<NEW>/' <FILE_NAME>  # replace first instance of "original" on each line of file with "new"
$ sed 's/<ORIGINAL>/<NEW>/g' <FILE_NAME> # replace all instances of "original" with "new" in file
```

---

### Permissions and Ownership
* `rwx`: `r`ead `w`rite `e`xecute
* When listed in triplet (ex. drwxrwxrwx)
  - `d`: directory  
  - first triplet: user permissions (`u`)  
  - second triplet: group permissions (`g`)  
  - third triplet: other permissions (`o`)  
* (`+`) to add permissions  
* (`-`) to remove permissions  
* (`=`) to make permissions specified the only permissions that file has  

```bash
$ ls -al                                        # list information for each file in the directory
$ chmod ug+rx <FILE_NAME>                       # give user and group read and execute permissions
$ chmod ugo-rwx <FILE_NAME>                     # remove all permissions from all users of group
$ chown <user> <FILE_OR_DIR_NAME>               # changes user ownership
$ chgrp <group> <FILE_OR_DIR_NAME>              # changes group ownership
$ chown <user>:<group> <FILE_OR_DIR_NAME>       # changes user and group ownership
```

---

### Slurm
* Quickstart: https://slurm.schedmd.com/quickstart.html
* Convenient Commands: https://docs.rc.fas.harvard.edu/kb/convenient-slurm-commands/
* Summary: http://www.physik.uni-leipzig.de/wiki/files/slurm_summary.pdf
```bash
$ sinfo --Node --long
$ sinfo --format=%G
$ sinfo -o "%n %e %m %a %c %C"

$ scontrol show node
$ scontrol show partition
$ scontrol show partition
$ scontrol show job <JobID>

$ squeue -o "%.18i %.9P %.15j %.8u %.8T %.10M %.9l %.6C %.5m %R" -u $USER
$ squeue --job <JobID>
$ squeue -u <username>

$ srun <options> --pty bash

$ sacct -u <username>

$ scancel <JobID>
```

---

### SSH

SSH Commands: https://man7.org/linux/man-pages/man1/ssh.1.html

```bash
$ ssh -q -o ServerAliveInterval=60 <hostname>
$ ssh-copy-id -i ~/.ssh/id_rsa.pub <username>@<hostname>.niaid.nih.gov -f
```

---

### tmux
* Cheat Sheet: https://tmuxcheatsheet.com/

```bash
$ tmux new -s <my_session_name>
$ tmux ls
$ tmux info
$ tmux kill-session -t <my_session_name>
$ tmux attach-session -t <my_session_name>
```

---

### Screen
Quick Commands: https://www.geeksforgeeks.org/screen-command-in-linux-with-examples/

```bash
$ screen -ls
```

---

## Conclusion

---

## See Also



---

* Cheat Sheets:
  * https://www.linux.org/threads/massive-collection-of-linux-command-cheat-sheet-for-2022.38934/
  * https://github.com/RehanSaeed/Bash-Cheat-Sheet
  * https://hpc.ua.edu/wp-content/uploads/2022/02/Linux_bash_cheat_sheet.pdf
  * https://devhints.io/bash

<details>
<summary><b>linux,command,reference,glossary,help,unix,hot dogs</b></summary>

</details>
