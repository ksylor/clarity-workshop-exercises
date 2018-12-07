# Creating and Cloning repos
Let's get started by cloning and creating git repos to work with. 

## Prework
Let's make sure that you have git installed on your laptop.

1. Pop open your favorite terminal or command line program 
-- in Mac the default is usually in the Finder under Applications -> Utilities -> Terminal. 
-- in Windows the default is usually in the Start Menu -> Command Prompt. Or, you can open the Start Menu and type `cmd` into the Search bar (or Run, depending on what version of windows), then hit enter.

2. type in the command `git --version`

3. Confirm that `git version x.x.x` is output. Ideally we want a version 2.x variant, if you have a 1.x version, let me know!.

## Create a new repo
1. Create a new folder on your laptop, call it whatever you want! Create some files in the new folder, they can be empty and named whatever you'd like. 

2. Copy the path to the new folder that you created. 

3. Pop open your favorite terminal or command line program and move into your new folder with the command `cd path/to/folder`

4. Confirm that you are in the correct folder using the command `pwd` on a mac or `cd` (no arguments) on windows. Then confirm that you have some files in there using the `ls` command on mac or `dir` on windows.

5. Initialize a new repository using the command `git init`. You should see the the output `Initialized empty Git repository in /path/to/folder/.git/`

6. Run the command `git add .` to add the new files to staging (we'll go over this in more detail later today).

7. Create the first commit in your repository using the command `git commit -m "Your text here"`. You should see output similar to this but with your file names:
```
[master (root-commit) 50e92cc] initial commit, hooray
 3 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 fun.txt
 create mode 100644 more-fun.txt
 create mode 100644 the-most-fun.txt
 ```

**Congratulations! you made a git repo!**

## Clone the example repo
