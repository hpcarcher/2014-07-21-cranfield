Branching
=========

`git branch servings` to create a branch called `servings`:

    -c1---c2---c3                               master *
                    \
                     .                          servings

`*` denotes the current branch.

`git checkout servings` to switch to `servings`:

    -c1---c2---c3                               master
                   \
                    .                           servings *

Commit changes on `servings`:

    -c1---c2---c3                               master
                \
                 c4                             servings *

    -c1---c2---c3                               master
                \
                 c4---c5---c6                   servings *

`git checkout master` to switch to `master`:

    -c1---c2---c3                               master *
                \
                 c4---c5---c6                   servings

Commit changes on `master`:

    -c1---c2---c3---c7---c8                     master *
                \
                 c4---c5---c6                   servings

`git checkout servings` to switch to `servings`:

    -c1---c2---c3---c7---c8                     master
                \
                 c4---c5---c6                   servings *

Commit changes on `servings`:

    -c1---c2---c3---c7---c8                     master
                \
                 c4---c5---c6---c9              servings *

Merges
------

`git checkout master` to switch to `master`:

    -c1---c2---c3---c7---c8                     master *
                \
                 c4---c5---c6---c9              servings

Commit changes on `master`:

    -c1---c2---c3---c7---c8---c10               master *
                \
                 c4---c5---c6---c9              servings

 `git merge servings` to merge changes from `servings` into `master`:

    -c1---c2---c3---c7---c8---c10---M1           master *
                \                 /
                 c4---c5---c6---c9               servings

Conflicts
---------

`git checkout servings` to switch to `servings`:

    -c1---c2---c3---c7---c8---c10---M1           master
                \                 /
                 c4---c5---c6---c9               servings *

Commit changes on `servings`:

    -c1---c2---c3---c7---c8---c10---M1            master
                \                 /
                 c4---c5---c6---c9---c11          servings *

`git checkout master` to switch to `master`:

    -c1---c2---c3---c7---c8---c10---M1            master *
                \                 /
                 c4---c5---c6---c9---c11          servings

Commit changes to same parts of files on `master`:

    -c1---c2---c3---c7---c8---c10---M1---c12      master *
                \                 /
                 c4---c5---c6---c9---c11          servings

 `git merge servings` to merge changes from `servings` into `master`:

`CONFLICT` warning is given and `Unmerged` files need to be edited to remove conflict markup:

    <<<<<<< HEAD

    =======

    >>>>>>> 71d34decd32124

`git add` to mark `Unmerged` files as having their conflicts resolved.

`git commit` to commit merge.

    -c1---c2---c3---c7---c8---c10---M1---c12---M2 master *
                \                 /           /
                 c4---c5---c6---c9---c11------    servings

Branching and software development
----------------------------------

Create a branch for a new feature, bug fix or a developer's own work:

    -c1---c2---c3                               master
                \
                 c4                             feature

Continue developing product:

    -c1---c2---c3---c5---c6---c7                   master *
                \
                 c4                                feature

Continue developing feature:

    -c1---c2---c3---c5---c6---c7                   master
                \
                 c4---c8---c9                      feature *

Merge feature into product when stable and tested:

     -c1---c2---c3---c5---c6---c7--c10              master
                \                   /
                 c4---c8---c9-------                feature

Continue developing product and feature:

    -c1---c2---c3---c5---c6---c7--c10---c11--c12     master
                \                /
                 c4---c8---c9-------c13              feature1

A popular model:

* Release branch - a released version of the code.
* Master branch - most up-to-date stable version of the code.
* Feature, bug and/or developer-specific branches.

Example:

               0.1      0.2        0.3
              c6---------c9------c17------               release
              /          /       /
        c1---c2---c3--c7--c8---c16--c18---c20---c21--    master
        |                      /
        c4---c10---c13------c15                          fred
        |                   /
        c5---c11---c12---c14---c19                       kate
