# Viewing and changing history

In this exercise, we'll practice different ways to move around in history, and how to change history when we make a mistake!

## Logs!

1. In the terminal, run the command `git log`. This will output a list of commits to your branch in order from newest up top to oldest. Each log entry should look like this:
```
commit f61ec7db4f963a437d9a72cb7b393a81f6a1efb0 (HEAD -> master)
Author: Katie Sylor-Miller <ksylormiller@etsy.com>
Date:   Sun Dec 9 17:59:49 2018 -0500

    my local changes
    
```
Use the up and down arrows to scroll through the log output. Type `q` to quit out of the log view.

2. Let's try using some other formats to see different information in the log. Run the command `git log --oneline` to see a shorter format for each log line. You can also do `git log pretty=<format>`, where you replace `<format>` with one listed here: https://git-scm.com/docs/pretty-formats. 

3. Now let's search the logs. We can search by Author (although in this case the output will be everything since you've made all the commits!) 
```
git log author="Katie" (or replace with your name!)
```
Another super useful way to search logs is by commit message using the `--grep` flag (grep is a search command from bash). 
```
git log --grep="my"
```
Just remember that the search is not super fuzzy. If I search for "My commit" then "My awesome commit" will not come back in the list of results.

## Viewing an entry in the log

1. We can take the log entries we just looked at and use `git show <commit hash>` to see more information about that commit. `show` will list out the log entry information _plus_ a diff of the changes in that commit.

2. To see what the entire repo looked like at a particular point in time we can use `git checkout <commit hash>` to move the HEAD to point to that commit in time. Remember that git will warn you that you are in a detached head state. Don't panic! It's totally fine. 

3. Let's say you want to make some new changes from this point in time. Run `git checkout -b branch-name` to create & checkout a branch from this point. Since `checkout -b` also checks out the new branch, you'll reattach your HEAD in the process.

4. Check back out the `master` branch. 

## Reflogs - our magic time machine

1. Let's take a look at the reflog by running `git reflog`. If the reflog is longer than the window height, you can move through it in the same way as regular log with the up and down arrows and typing `q` to quit.
