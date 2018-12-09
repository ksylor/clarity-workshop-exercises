# Workflow

In this exercise we'll practice some common workflow operations and get more familiar with the different environments in git.

## Modify files and add to staging/index

Let's practice modifying files in the command line using vim, then adding those files to staging.

1. Open up the terminal/command line and make sure you have the master branch checked-out in the repo you created.

2. Choose any file in your example repo to edit, and open it up using vim by typing the command `vim <filename>`.

3. Type `i` to put vim into edit mode.

4. Use the up and down arrows to navigate around the file, add some new lines and/or change some existing lines of text.

5. When you are ready to save, hit `esc` to go back into command mode. then colon `:` `w` `q` and `enter` to tell vim you want to write out the file and quit. 

6. Run the command `git diff`. You should see something like this for output:

```diff
diff --git a/fun.txt b/fun.txt
index ee2280c..21b7d7d 100644
--- a/fun.txt
+++ b/fun.txt
@@ -1,2 +1,3 @@
 Hi this is candy I'm having fun!
 Are you having fun yet?
+Yes I am having so much fun.
```

7. Run the command `git status`. You should see something like this:
```
On branch master
Your branch is up-to-date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   fun.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

8. Follow steps 2-5 using a different file. If you only have one file in your repo, running the command `vim <new-filename>` will both create and open a new file.

9. Run `git diff` and `git status` again to confirm that you've changed both files. 

10. Choose one of the two files that you've changed and add it to the staging area with `git add <filename>`.

11. Run `git status` again. Now you should see one file in "Changes to be committed" - AKA in staging, and the other file in "Changes not staged for commit" aka the workspace.
```
On branch master
Your branch is up-to-date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   fun.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   the-most-fun.txt
```
  
12. Run `git diff` again. The file in staging dissapeared! Run `git diff --staged` (or the alias `git diff --cached` does the same thing) to see the file in staging. Run `git diff HEAD` to see both files.

13. What happens if you edit the file in staging, then run `git status` and `git diff` again? 

14. Commit _just_ the file in staging with `git commit -m "Your message here"` 

15. Run `git status` again. The changes in staging are gone, they've been committed. But your workspace changes are still there. 

16. Add and commit the remaining changes. 

17. Now if we run `git status` we should see something like
```
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```
18. Let's push those changes up to origin! Run `git push`. You should see something like:

```
Counting objects: 6, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (6/6), 580 bytes | 580.00 KiB/s, done.
Total 6 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 1 local object.
To https://github.com/ksylor/clarity-workshop-example.git
   995198f..39b50af  master -> master
```

19. Now if we run `git status` again, we should be up to date!
```
On branch master
Your branch is up-to-date with 'origin/master'.

nothing to commit, working tree clean
```
