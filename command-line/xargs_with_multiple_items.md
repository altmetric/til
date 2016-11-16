# Working on multiple items with `xargs`

When processing or reprocessing work, we often have a list of identifier IDs that need to be processed in the same way – for example, all being sent to the same Redis-backed queue.

Assuming that the IDs are available in a text file with one ID per line, one _could_ edit the file using a text editor's multi-line mode to create a series of shell commands. For example, if our source file was:

```
1234
1235
1237
1265
1836
2462
2347
```

We could convert it to:

```
redis-cli LPUSH my_queue_name 1234
redis-cli LPUSH my_queue_name 1235
redis-cli LPUSH my_queue_name 1237
...
```

and then run the resulting script file.

More effective, though is to use the `xargs` command, which can take each line of the file and append it as an argument to a supplied command. This will issue commands one by one, effectively performing the same tasks as if we'd created that script file – but it will work for a list of IDs of any length without needing to edit anything:

```bash
% cat my_id_list.txt | xargs redis-cli LPUSH my_queue_name
# equivalent to
# redis-cli LPUSH my_queue_name 1234
# redis-cli LPUSH my_queue_name 1235
# redis-cli LPUSH my_queue_name 1237
#...
```

## `-L` for greater efficiency

With redis in particular, it is possible to be even more efficient, since the `LPUSH` command can take multiple items to be pushed in a single line. `xargs` has an `-L` argument which will append a given number of items from the source file. For example:

```bash
% cat my_id_list.txt | xargs -L 5 redis-cli LPUSH my_queue_name
# equivalent to
# redis-cli LPUSH my_queue_name 1234 1235 1237 1265 1836
# redis-cli LPUSH my_queue_name 2462 2347...
```

While grouping by a small number doesn't bring us many benefits, there are noticeable speed improvements when pushing hundreds, or even thousands, of IDs in a single line.

### Further reading

* [xargs man page](http://man7.org/linux/man-pages/man1/xargs.1.html) (or `man xargs` on command line)
* [Redis LPUSH command](http://redis.io/commands/lpush)
