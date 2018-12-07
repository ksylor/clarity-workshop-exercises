# Exercise 2 - Commits, Branches, Refs and HEAD
Let's learn more about some fundamental data structures

## Prework
Set up your terminal/command line to show the current directory and the currently checked out git branch. (image)

On Mac and Unix machines, the terminal is actually a *bash shell* program, which allows you to running commands that interact directly with your computer. Instructions are written in a language called bash. 

We can customize our bash environment to do all kinds of cool stuff using what's called a bash profile. The bash profile is a helper script that gets run whenever you open up a bash shell like the terminal (or iterm, etc.). So, let's use a pre-built script to show us some useful information and color-code it.

1. In the command line, run the command `vim ~/.bash_profile`. This will either open the existing file in the Vim editor, or it will create the file if it doesn't exist. Vim is super confusing if you've never used it before, but we can get through this!

2. When Vim opens a new file, it starts in a read-only mode. You cannot modify the file, only look at it. Hit the `I` key to enter vim's `insert` mode. This will allow you to enter text. You will see `-- INSERT --` at the bottom of the window. (image)

3. Copy/paste the following block of code into the bash_profile file. If you already have stuff in your bash_profile, use the down arrow key until you are at the end of that text, then hit enter to move onto a new line before you paste. 

```
function parse_git_branch () {
  git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}

RED="\[\033[0;31m\]"
YELLOW="\[\033[0;33m\]"
GREEN="\[\033[0;32m\]"
NO_COLOR="\[\033[0m\]"

PS1="$GREEN\u@\h$NO_COLOR:\w$YELLOW\$(parse_git_branch)$NO_COLOR\$ "
```

4. Hit the escape key to exit `insert` mode. The bottom line will be blank.

5. Type a colon `:` by hitting `shift + ;` this gets vim ready to accept an operational command.

6. Tell vim that you want to **write** the contents of the file (e.g. save the changes you made), then **quit** the program by typing `w` then `q` then hit `enter` to execute the commands.

**Woooo! you successfully quitted out of vim!**

7. Now we need to tell your current shell session that we want to run and use that command. we do that with the command `source ~/.bash_profile`

8. Hopefully this has changed your bash prompt, if not, let me know.

