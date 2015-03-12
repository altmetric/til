# Moving recent commits to another branch

If you have accidentally made one or more consecutive commits on the wrong
branch, e.g. `master`, you can do the following to move those commits to a new
branch (here named `new-branch-with-commits-preserved`):

```console
$ git branch new-branch-with-commits-preserved
$ git reset --hard SOMESHA
$ git checkout new-branch-with-commits-preserved
```

The trick here is that `git branch` with a name creates a branch without
checking it out. This means that when you `reset` to your last good `SOMESHA`
(replace with whatever you want to reset your current branch to), you are
actually removing the work from your current branch while leaving the new
branch intact.
