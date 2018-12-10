# Viewing and changing history

In this exercise, we'll practice different ways to move around in history, and how to change history when we make a mistake!

## View the history of a branch

Let's see how we can use `git log` to view the history of a branch.

1. In the terminal, run the command `git log`. This will output a list of commits to your branch in order from newest up top to oldest. Each log entry should look like this:
```
commit f61ec7db4f963a437d9a72cb7b393a81f6a1efb0 (HEAD -> master)
Author: Katie Sylor-Miller <ksylormiller@etsy.com>
Date:   Sun Dec 9 17:59:49 2018 -0500

    my local changes
    
```
Use the up and down arrows to scroll through the log output. Type `q` to quit out of the log view.

2. Let's try using some other formats to see different information in the log. Run the command `git log --oneline` to see a shorter format for each log line. You can also do `git log --pretty=<format>`, where you replace `<format>` with one listed here: https://git-scm.com/docs/pretty-formats. 

3. Now let's search the logs. We can search by Author (although in this case the output will be everything since you've made all the commits!) 
```
git log --author="Katie" (or replace with your name!)
```
Another super useful way to search logs is by commit message using the `--grep` flag (grep is a search command from bash). 
```
git log --grep="my"
```
Just remember that the search is not super fuzzy. If I search for "My commit" then "My awesome commit" will not come back in the list of results.

## View the history of the HEAD (our magic time machine)

Let's see how we can view the history of the actions we take in git with the reflog. Then we can use the reflog to undo actions we've taken to save ourselves from mistakes. 

1. Run the command `git reflog`. You'll see a list of all of the actions you've taken which affect your local HEAD. Here's what my last 10 entries look like.
```
39b50af (HEAD -> past-branch) HEAD@{0}: checkout: moving from 39b50afeb8e954421105bff3cf1fbeff570fa804 to past-branch
39b50af (HEAD -> past-branch) HEAD@{1}: checkout: moving from master to 39b50afeb8e954421105bff3cf1fbeff570fa804
f61ec7d (master) HEAD@{2}: rebase finished: returning to refs/heads/master
f61ec7d (master) HEAD@{3}: pull --rebase --stat: my local changes
9248d43 (origin/master) HEAD@{4}: pull --rebase --stat: checkout 9248d4379993ca240dd4e5898a0a9dd2e3ab0efc
09a1663 HEAD@{5}: checkout: moving from newer-branch to master
f1321d7 (origin/feature-branch, newer-branch, feature-branch) HEAD@{6}: checkout: moving from feature-branch to newer-branch
f1321d7 (origin/feature-branch, newer-branch, feature-branch) HEAD@{7}: commit: Need another commit
5921687 HEAD@{8}: checkout: moving from master to feature-branch
09a1663 HEAD@{9}: checkout: moving from feature-branch to master
```

Each entry lists out a short hash that represents that state, then some information about the current branch (if applicable), then a `HEAD@{index}` notation where `0` is the most recent action, and the index number goes up each step back in time. 

## Viewing more about a point in time

1. We can take the log entries we just looked at and use `git show <hash>` or `git show HEAD@{x}` notation to see more information about that commit. `show` will list out the log entry information _plus_ a diff of the changes in that commit.

2. To see what the entire repo looked like at a particular point in time we can use `git checkout <hash>` to move the HEAD to point to that commit in time. Remember that git will warn you that you are in a detached head state. Don't panic! It's totally fine. 

3. Let's say you want to make some new changes from this point in time. Run `git checkout -b branch-name` to create & checkout a branch from this point. Since `checkout -b` also checks out the new branch, you'll reattach your HEAD in the process.

4. Check back out the `master` branch. 

## Undoing commits
First, let's undo the last commit on our master branch. Normally we wouldn't use reset to change master since we've pushed the most recent changes to the branch to a public repo (and you should never change public history!), but because these are not shared with anyone else we can break the rules a bit! 

1. There are a few different ways we can undo the last commit. Both involve resetting to the point _before_ the last commit. Use whichever you like!
- run `git log` to look up the hash of the second-to-last commit on the branch, then `git reset <hash>`
- run `git reset HEAD~` which is shorthand for "go back one commit from the current HEAD"

(Note: you probably don't want to use the reflog `HEAD@{x}` notation for this, unless you don't care about preserving all of the actions between now and then.)

2. You should see the output: 
```
Unstaged changes after reset:
M	the-most-fun.txt
```

This is because the default behavior of `reset` is `--mixed`, which leaves our changes in the workspace. You can confirm this with `git status`.

3. If we really want to just get rid of this work without saving it, we can run `git reset --hard` to YOLO and throw away our changes from the workspacew. You _can_ get it back, but always use caution with a hard reset just to be safe.

4. Run `git log` again to confirm that the commit is no longer in the history of the branch. 

5. If we now want to push up this state of our master branch to the remote repo, git will stop us! 
```
To https://github.com/ksylor/clarity-workshop-example.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/ksylor/clarity-workshop-example.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

If we are really really really sure that yes, we don't care about losing those commits in the public/shared remote repo, we can tell git to go ahead with the force flag `git push --force`. 

## Undoing actions
Now, let's undo some of our prior actions using `git reset` and the reflog. This is useful for situations where you really mess up the state of your branch, and simply undoing the last commit or last few commits isn't enough. It is the only option to undo a rebase action, since rebasing re-writes the history of a branch. We could also use reset + reflog to get back that commit we threw away! 

1. run `git reflog` again, and find a HEAD entry from before a destructive action like a rebase or a reset, then reset back to that state. In my case I will run `git reset HEAD@{5}`. 

2. run `git reflog` again. You can see that we've pushed another entry into the reflog that represents our new `reset` action. That's the nice thing about the reflog, it doesn't throw away the prior state, but keeps it around for you - although not indefinitely!

## Undoing commits in a public branch
What about a case where we want to undo a commit that is surrounded by commits we want to keep, or if we want to undo a commit from a public, shared branch? For this we use `revert`.

1. Run `git log` again, and pick a commit hash from a few commits back to un-do.

2. Run `git revert <hash>`. This will open up vim with an auto-generated commit message that looks something like this:
```
Revert "Newer branch (#5)"

This reverts commit 9248d4379993ca240dd4e5898a0a9dd2e3ab0efc.

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# Your branch is up-to-date with 'origin/master'.
#
# Changes to be committed:
#       modified:   fun.txt
#
~
~
~
~
"~/clarity-workshop-example/.git/COMMIT_EDITMSG" 13L, 350C
```

3. This looks fine so use `:wq` or `:x` to save & quit out of vim

4. Run `git log` again. You will see a new commit at the tip of the branch _and_ the old commit, still in history. 

5. Run `git show <new commit hash>` and compare that to `git show <reverted commit hash>`, the new commit should be exactly the opposite patch as the reverted commit - so we've un-done that commit.

6. Now you can do a regular `git push` to get that revert into your public repo without worrying that you've messed up anyone else.

**Congratulations! You can now fix a whole bunch of mistakes in git!**
