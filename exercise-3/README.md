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

## Merge vs Rebase Commits

Now we'll practice making merge and rebase commits.

1. First, we need to create a conflict. Open up your fork of the example repo in github.com

2. Click on `fun.txt` and click the pencil icon to edit it

3. Change some text in the file. Enter "Their changes" as the commit message, select "Commit directly to the `master` branch", and click "Commit changes"

4. Go back to your example repo in your terminal. _Do not `git pull` or `git fetch` yet_. Make sure you have the `master` branch checked out.

5. Open up `more-fun.txt` in vim by typing the command `vim more-fun.txt`.

6. Change the contents of `more-fun.txt`. Refer to the earlier exercise if you've forgotten the vim hotkeys.

7. Stage, then commit your changes to `more-fun.txt`. That is, `git add more-fun.txt` and `git commit -m "My changes"`.

8. Run `git status`. You should see something like:
```
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

9. As you can see, git thinks that we can fast-forward, but that's because git doesn't know about the conflicting changes on github yet.

10. Run `git fetch`, then `git status`. You should now see something like:
```
On branch master
Your branch and 'origin/master' have diverged,
and have 1 and 1 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)

nothing to commit, working tree clean
```

11. What happens if you try to `git push`? Try it! You'll see something like:
```
git push
To github.com:ben-ng/clarity-workshop-example.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'git@github.com:ben-ng/clarity-workshop-example.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

12. Now we'll resolve this the ugly way -- with a merge commit. Run the suggested `git pull`, which if you recall, is equivalent to `git fetch && git merge`. You'll get a vim editor with the commit message "Merge branch 'master' of github.com:ben-ng/clarity-workshop-example". Approve it with `:x`. You'll see something like this:
```
Merge made by the 'recursive' strategy.
 fun.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

13. Take a look at the last three commits with `git log -3`. You'll see our merge commit, and its two parent commits in the `Merge: 1783994 ac38953` line:
```
commit 61fde39511154b9f6324f960ada730bfcb0dcb1d (HEAD -> master)
Merge: 1783994 ac38953
Author: Ben Ng <me@benng.me>
Date:   Sun Dec 9 13:36:14 2018 -0500

    Merge branch 'master' of github.com:ben-ng/clarity-workshop-example

commit 17839947c614dcf30740ebb45dbc572bb6666556
Author: Ben Ng <me@benng.me>
Date:   Sun Dec 9 13:34:17 2018 -0500

    Our changes

commit ac389533738c9965a37ded0613ffa5295cfb7505 (origin/master, origin/HEAD)
Author: Ben Ng <me@benng.me>
Date:   Sun Dec 9 13:22:06 2018 -0500

    Their changes
```

14. Now let's undo this merge operation with `git reset --hard ORIG_HEAD`. Verify with `git log` and `git status` that it worked:
```
On branch master
Your branch and 'origin/master' have diverged,
and have 1 and 1 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)

nothing to commit, working tree clean
```

15. This time, we'll resolve the conflict by rebasing. Run `git pull --rebase`, and you'll see something like this:
```
First, rewinding head to replay your work on top of it...
Applying: Our changes
```

16. This time, if you take a look at `git log -3`, you'll only see `Our Changes` and `Their Changes`. No more nasty merge commit!
```
commit bcdc48757175ebcfc96baf4fe460df2252f33eff (HEAD -> master)
Author: Ben Ng <me@benng.me>
Date:   Sun Dec 9 13:34:17 2018 -0500

    Our changes

commit ac389533738c9965a37ded0613ffa5295cfb7505 (origin/master, origin/HEAD)
Author: Ben Ng <me@benng.me>
Date:   Sun Dec 9 13:22:06 2018 -0500

    Their changes
```

17. `git status` will report that you can now fast-forward on `git push`:
```
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

18. Run `git push`. You'll see something like:
```
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 257 bytes | 257.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:ben-ng/clarity-workshop-example.git
   ac38953..bcdc487  master -> master
```

19. You're all done! Verify with `git status`:
```
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```
