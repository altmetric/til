# Changing a file's case on macOS

If you have accidentally named a file with uppercase when it should have been lower or vice versa, it can be hard to convince Git that you have made a change if you're working on macOS.

This is because by default, macOS drives are **case preserving**, but **case insensitive**. So in the operating system's eyes, variations in spelling do not count as changes to a file. (You can also refer to files using any case you like, so that `open README.md` and `open Readme.md` will both work on a file which shows up as `readme.md`.)

This makes it tricky to `git add` a change, because the operating system doesn't perceive there being any change to make.

A workaround is to perform the rename in two steps, staging each step but not committing until the very end:

1. Rename your source file (let's call it `SOURCE_FILE.rb`) to a temporary name that is materially different (in more than just its case):

        $ mv SOURCE_FILE.rb source_file_cased.rb

2. `git add` this change, which should show up in a `git status` as a rename:

        $ git add .
        $ git status

        Changes to be committed:
          (use "git reset HEAD <file>..." to unstage)

          renamed:    SOURCE_FILE.rb -> source_file_cased.rb

3. Rename the new file to the case-changed version, and stage this change too:

        $ mv source_file_cased.rb source_file.rb
        $ git add .
        $ git status

        Changes to be committed:
          (use "git reset HEAD <file>..." to unstage)

          renamed:    SOURCE_FILE.rb -> source_file.rb

4. Commit and continue as usual.

## Even shorter...

```bash
$ git mv SOURCE_FILE.rb source_file_cased.rb
$ git mv source_file_cased.rb source_file.rb
$ git commit
```
