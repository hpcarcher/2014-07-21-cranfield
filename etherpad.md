EtherPad notes
==============

Notes from the boot camp EtherPad. 

Please see the scripts in [scripts](./scripts) for commands, scripts
and code that were typed and run during the sessions.

Bash
----

To capture output (stdout) and errors (stderr) into the same file:

    $ COMMAND > output.txt 2>&1

To capture output (stdout) and errors (stderr) into separate files:

    $ COMMAND 2>outputerror.txt >output.txt

An alternative to backticks `$(COMMAND)`. The following are identical:

    $ FILES=`ls *`
    $ FILES=$(ls *)

Backticks are deprecated:

 * [StackOverflow](http://stackoverflow.com/questions/4708549/shell-programming-whats-the-difference-between-command-and-command)
 * [StackOverflow](http://stackoverflow.com/questions/9449778/what-is-the-benefit-of-using-instead-of-backticks-in-shell-scripts)

Loops can be typed on a single line e.g.:

    $ for FILE in $FILES; do wc $FILE; done

Or, on multiple lines e.g.:

    $ for FILE in $FILES;
    $ do wc $FILE;
    $ done

From the `script` manual page:

    $ man script

    script is used to take a copy of everything which is output to
    the terminal and place it in a log file. It should be followed by
    the name of the file to place the log in, and the exit command
    should be used to stop logging and close the file.  

To empty / clear / unassign a shell variable:

    $ unset VARIABLE_NAME

An alternative to `wget` is `curl`. Some operating systems that don't
have `wget` (e.g. Mac Maverick) may have `curl`.

The following both fetch a JPG from a URL:

    $ wget http://www.cookuk.co.uk/images/slow-cooker-winter-vegetable-soup/smooth-soup.jpg
    $ curl http://www.cookuk.co.uk/images/slow-cooker-winter-vegetables-soup/smooth-soup.jpg -o smooth-soup.jpg

Convert DOS output files to Unix format in `git bash`:

    $ dos2unix.exe FILENAME
    $ dos2unix.exe flow.dat

Git
---

Show log messages using one line each:

    $ git log --pretty=oneline

Or:

    $ git log --oneline

Show log messages using one line each with tags:

    $ git log --pretty=oneline --decorate

Add colour to Git's outputs:

    $ git config --global color.ui auto

Delete branch only if it has been merged

    $ git branch -d BRANCHNAME

Delete branch whether or not it has been merged:

    $ git branch -D BRANCHNAME

Git can be used with a directory that is held within DropBox or Google
Drive. There might be issues when trying to push or pull when DropBox
or Google Drive are synching though.

If you have added a file to the Git staging area, using `git add` then you can remove it again using `git reset` e.g. after changing `sample.txt`:

    # Add sample.txt to the staging area.  
    $ git add sample.txt

    # Change your mind and remove it from the staging area.
    $ git reset sample.txt

See [StackOverflow](http://stackoverflow.com/questions/348170/undo-git-add-before-commit).

There is also `git rm --cached filename` but this requires care in remembering the `--cached` flag.

`.gitignore`
------------

You can create a `.gitignore` file which lists the patterns of files
you want Git to ignore. It's common practice to not add to a
repository any file you can automatically create in some way e.g. C
object files (`.o`), Java class (`.class`) files or temporary files
e.g. XEmacs scratch files (`~`). Adding these to `.gitignore` means
Git won't complain about them being untracked.

For example, is we have a repository `cookbook/` then we would do:

    $ cd cookbook
    $ ls -a
    .git

Then we'd create a `.gitignore` file:

    $ nano .gitignore

To this we would add patterns for the files you want to ignore, where
`*` is a wildcard:

    *~
    *.o
    *.so
    *.dll
    *.exe
    *.class
    *.jar

There is also a very tidy way to ignore many different unwanted files
with `!` sign that stands for "not ignore" (e.g. `!*.C` means - don't
ignore files that end with `.C`). 

For example, when you compile LaTeX files you usually want to track
with git only source files with (`*.tex`) extension ignoring all other
files created during the compilation process. 

You can write a `.gitignore` file as follows:

    *
    ! *.tex

Git will interpret this as: ignore everything but keep tracking `.tex`
files. It is much more concise than having a `.gitignore` file with:

    *.aux
    *.ps
    *.pdf
    *.log

We would add `.gitignore` to our repository as follows:

    $ git add .gitignore
    $ git commit -m "Added rules to ignore XEmacs scratch files and binary files"

Branching example
-----------------

A small branching example with merging alternatives.

Create small repository:

    $ mkdir branchsample
    $ cd branchsample
    $ git init

Create example file:

    $ cat > sample.md
    1
    2
    3
    CTRL-D
    $ add sample.md
    $ git commit -m "First commit" sample.md

Create branch and make changes:

    $ git branch mybranch
    $ git checkout mybranch
    $ cat > sample.md
    P
    Q
    R
    CTRL-D
    $ git commit -m "Changed 123 to PQR" sample.md

    Return to master and make changes:

    $ git checkout master
    $ cat > sample.md
    A
    B
    C
    CTRL-D
    $ git commit -m "Changed 123 to ABC" sample.md

Merge branch:

    $ git merge mybranch
    Auto-merging sample.md
    CONFLICT (content): Merge conflict in sample.md
    Automatic merge failed; fix conflicts and then commit the result.

Show common ancestor:

    $ git show :1:sample.md
    1
    2
    3

Show `HEAD` version, version in current branch, `master`:

    $ git show :2:sample.md
    A
    B
    C

    Show `MERGING_HEAD` version, version in other branch, `mybranch`:

    $ git show :3:sample.md
    P
    Q
    R

Now we can...

EITHER, resolve conflict by manually editing sample.md then resolving
the commit by doing: 

    $ git add sample.md
    $ git commit -m "Merged from mybranch and blended together both versions"

OR, if I want to keep the `master` version and ignore the branch
version: 

    $ git checkout --ours sample.md
    $ git add sample.md
    $ git commit -m "Merged from mybranch, and kept master version"

Instead of `git checkout --ours` we can use `git checkout -2 sample.md`.

OR, if we want to keep the branch version and ignore the `master`
version: 

    $ git checkout --theirs sample.md
    $ git add sample.md
    $ git commit -m "Merged from mybranch, and kept mybranch version"

Instead of `git checkout --ours` we can use `git checkout -3 sample.md`.

When to keep the `master` version, or the branch version, or a mixture
of the two is very much dependant upon the work that is done. 


Is there a way to automatically enter username and password for bitbucket/github?
not AFAIK, but you can set it to remember it for a time period.
"git config --global credential.helper cache"
will get it to remember your password for (I think) 15 minutes, so you have to type it less often.

Make
----

Syntax:

    # Comment

    target : dependency dependency
    [tab] rule
    [tab] rule

    # Target with no dependencies:
    target :
    [tab] rule
    [tab] rule

    # Target with no rules:
    target : dependency dependency

Scientific Programming in Python
--------------------------------

Viewing images via `ipython`:

    $ ipython -pylab
    In [1]: import matplotlib.image as mpimg
    In [2]: import matplotlib.pyplot as mpplt
    In [3]: img = mpimg.imread('smooth-soup.png')
    In [4]: mpplt.imshow(img)

Testing
-------

If you are running Git Bash and nosetests gives `command not found`, do the following (replacing `USERNAME` with your username):

    $ alias nosetests='python /c/Users/USERNAME/Anaconda/Scripts/nosetests-script.py'

Documenting code
----------------

[Doxygen](http://www.stack.nl/~dimitri/doxygen/) is a free, and widely-used, tool that supports a notation for commenting C, C++, FORTRAN, Python and other code - in particular function, sub-routine and method arguments and return types. Once the code has been commented using the Doxygen notation, the Doxygen tool will parse this into HTML or LaTeX. This is useful if you want to share documentation about your modules, classes, functions, subroutines, methods and their arguments and return types, what they do and how they behave. This can help people understand what the code does and how to use it in their own code without having to look at the source code.

Java has it's own, equivalent, tool, [JavaDoc](http://www.oracle.com/technetwork/java/javase/documentation/index-jsp-135444.html).
