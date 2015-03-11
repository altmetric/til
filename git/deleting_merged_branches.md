# Deleting merged branches

Over time, my local git repository collects a series of branches that have been merged into `master`. If I attempt to clean them up, it's not immediately clear which branches have been merged in (and can therefore be safely deleted) and which haven't (and which could possibly contain code that would be lost upon deletion).

TIL that Git can do this very quickly.

```bash
# Lists all branches which are reachable from the currently checkout node,
# removing the current branch (prefixed with a '*'),
# then deletes each branch
git branch --merged | grep -v "\* master" | xargs -n 1 git branch -d
```

_Thanks to [@shamess]. See also: [Steven Harman's blog](http://stevenharman.net/git-clean-delete-already-merged-branches)_

[@shamess]: http://github.com/shamess
