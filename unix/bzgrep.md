# Working with compressed logs

Most times, you'll find that logs have been compressed when they're rotated. This makes it incredibly tricky to `grep` through them to find log line you're looking for - you'd first have to decompress them.

However, there is a command which will grep through the compressed files for you: `bzgrep`.

```
$ ls
altmetric.log  altmetric.log.14.bz2  altmetric.log.19.bz2  altmetric.log.23.bz2  altmetric.log.28.bz2
$ bzgrep 'Failed to get any items from feed 1908.' ./*
        altmetric.log.14.bz2:[2017-03-15 20:59:55] altmetric.WARNING: Failed to get any items from feed 1908. Skipping. [] []
```

You might also be interested in `bzcat`, `bzless`, or `bzmore`.

There are similar tools for other compression types (like `zgrep`), so double-check before going to the effort of decompressing.
