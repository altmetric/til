# `git commit --patch`

When striving to make atomic commits (so that you can [tell stories with your
Git
history](https://about.futurelearn.com/blog/telling-stories-with-your-git-history/)),
you might often find yourself using the following workflow:

```console
$ vim foo       # make changes
$ git add -p    # interactively select the patches you want to commit
$ git commit -v # commit your changes
```

If so, you can avoid using [`git-add`](http://git-scm.com/docs/git-add) by
passing `-p` or `--patch` directly to
[`git-commit`](http://git-scm.com/docs/git-commit) itself:

```console
$ vim foo       # make changes
$ git commit -p # interactively select patches and then commit
```
