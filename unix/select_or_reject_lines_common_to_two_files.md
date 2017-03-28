# Select or reject lines common to two files `comm`

Oftentimes we may want to compare two **sorted** files to get lines missing from one of them.

For example, let us say that we've got these two files:

```bash
% cat file_1
Auror office
Department of Magic
Department of Mysteries
Goblin Liaison Office

% cat file_2
Auror office
Department of Magic
Department of Mysteries
Department of Magical Law Enforcement
```

We may wonder which departments are common for both files. To do that we may use `comm` like so:

```bash
% comm -12 file_1 file_2
Auror office
Department of Magic
Department of Mysteries
```

If we want to only get the departments that occur in the first file we'd do:

```bash
% comm -23 file_1 file_2
Goblin Liaison Office
```

or this to only get departments in the second file:

```bash
% comm -13 file_1 file_2
Department of Magical Law Enforcement
```

### Further reading

* [comm man page](https://linux.die.net/man/1/comm) (or `man comm` on command line)
