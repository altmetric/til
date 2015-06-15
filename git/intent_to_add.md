# Intent to add

`git add -p` (or `git add --patch`) allows you to interactively choose bits of
work to add to the index but does not work on new files:

```console
$ git add -p some-new-file
No changes.
```

However, you can use `git add -N` (or `git add --intent-to-add`) to record a
new file and then use `git add -p` as normal.

```console
$ git add -N some-new-file
$ git add -p some-new-file
diff --git a/some-new-file b/some-new-file
index decafbad..deadbeef 100644
--- a/some-new-file
+++ b/some-new-file
@@ -0,0 +1 @@
+Huzzah!
Stage this hunk [y,n,q,a,d,/,e,?]?
```

See the [`git-add` documentation](http://git-scm.com/docs/git-add) for more information.
