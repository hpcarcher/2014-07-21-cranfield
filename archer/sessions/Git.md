Version Control and Git
=======================

Introduction
============

Motivation:

* ["FINAL".doc](http://www.phdcomics.com/comics/archive.php?comicid=1531)
* [A story told in file names](http://www.phdcomics.com/comics/archive.php?comicid=1323)
* You back-up a file only to realise later the back-up was a bugged or corrupted version.
* You are asked for the script used to create data graphed in a conference paper.
* A reviewer asks for a figure to be redone more clearly with exactly the same code, configuration and data.
* Your laptop is stolen!
* A colleague, since left, has rewritten key functionality and you want to know why. 

Version control AKA revision control preserves history of changes made to files or directories.

* Track Changes in Microsoft Word or file versions in DropBox or Google Drive.
* [Wikipedia](http://en.wikipedia.org/wiki/Main_Page).
* Changes to sets of files with reasons why.
* Source code, scripts, build files, configuration files, parameter sets, data files, documentation, papers, chapters etc.
* CVS, Subversion, Mercurial, [Git](http://git-scm.com/).
* Git is a version control tool. GitHub is a project hosting infrastructure that uses Git.

Tracking changes with a local repository
========================================

Create a local repository
-------------------------

Make sure you are not in any clone of the boot camp materials!

    git
    git --version
    git help
    git help --all
    git help checkout
    mkdir cookerybook
    cd cookerybook
    git init  # Create repository
    ls .git   # Git's configuration files - DO NOT TOUCH!

Configure Git locally
---------------------

    git config --global user.name "Your Name"  # Update configuration applied to all repositories
    git config --global user.email "yourname@yourplace.org"
    cat ~/.gitconfig

    git config --global core.editor nano  # Set default editor. Alternatives are vi, xemacs,...
    git config -l                         # Alternative to cat ~/.config

Add files and directories and record changes
--------------------------------------------

Create a file `recipe.md` in [Markdown](http://daringfireball.net/projects/markdown/syntax) syntax.

Add Ingredients and Cooking Instructions.

    git status  # Current status of files in repository.

Current directory is called the 'working directory'.

`Untracked` - files in working directory that Git is not managing.

    git add recipe.md     # Add file to staging area (AKA index or cache or loading dock)
    git status recipe.md  # Can check status to whole directory or specific files.

`Changes to be committed` - content in the staging area, ready for Git to manage.

    git commit  # Commit change to repository.

Provide a commit message, the "why":

* Git deduces what was changed and when, and, using the configuration, who by. 
* Messages like "made a change" or "commit 5" are redundant.
* Good commit messages usually have a one-line description followed by a longer explanation.

Git shows number of files changed and the number of lines inserted or deleted across all those files.

    git status recipe.md

`nothing to commit` - Git is managing everything in the working directory.

    git log

'Commit identifier' AKA 'revision number' uniquely identifies changes made in this commit, author, date, and commit message.

    git log --relative-date

Make updates to `reciie.md`.

    git status recipe.md

`Changes not staged for commit` section and `modified` marker - file managed by Git has been modified and changes are not yet committed.

    git add recipe.md
    git commit

Good commits are atomic and consist of the smallest change that remains meaningful. 

Commit changes that can be reviewed by someone else in under an hour.:

* Fagan (1976) discovered that a rigorous inspection can remove 60-90% of errors before the first test is run. M.E., Fagan (1976). [Design and Code inspections to reduce errors in program development](http://www.mfagan.com/pdfs/ibmfagan.pdf). IBM Systems Journal 15 (3): pp. 182-211.
* Cohen (2006) discovered that all the value of a code review comes within the first hour, after which reviewers can become exhausted and the issues they find become ever more trivial. J. Cohen (2006). [Best Kept Secrets of Peer Code Review](http://smartbear.com/SmartBear/media/pdfs/best-kept-secrets-of-peer-code-review.pdf). SmartBear, 2006. ISBN-10: 1599160676. ISBN-13: 978-1599160672.

Create a directory:

    mkdir images
    cd images

Download image of recipe from the web and put into `images` or use `wget`:

    wget http://www.cookuk.co.uk/images/slow-cooker-winter-vegetable-soup/smooth-soup.jpg

Add directory to repository:

    git add images
    git commit -m "Added images directory and soup image" images

Add link to recipe:

    [Soup](images/smooth-soup.jpg "My soup")

Commit:

    git commit -m "Added link to image of my soup." recipie.md

What to commit to the repository:

* Anything that is created manually e.g. source code, scripts, Makefiles, plain-text documents, notes, LaTeX documents, configuration files, input files.
* This can include Word or Excel.

What not to commit to the repository:

* Anything created automatically by a compiler or tool e.g. object files, binaries, libraries, PDFs etc.
* These can be recreated from sources.
* Reduces risk of auto-generated files becoming out-of-synch with the sources they are created from.

View differences
----------------

Make and commit changes to `recipe.md`.

    git diff recipe.md

* `-` - a line was deleted. 
* `+` - a line was added. 
* A line that has been edited is shown as a removal of the old line and an addition of the updated line.

Discard changes
---------------

    git checkout -- recipe.md  # Throw away local changes and 'revert'.
    git status data-report.md

Look at history
---------------

Make and commit changes to `recipe.md`.

    git log
    git log recipe.md
    git diff COMMITID
    git diff OLDER_COMMITID NEWER_COMMITID
    git log
    git checkout COMMITID  # Roll-back working directory to state of repository at first commit.
    ls
    cat recipe.md
    git checkout master  # Return to current state.
    ls

'undo' and 'redo' for directories and files.

* Commit early, commit often, increase granularity of undo/redo.
* If you make a mistake e.g. check in buggy code then the older versions are still there.
* DropBox and GoogleDrive preserve versions, but delete old versions after 30 days, or, for GoogleDrive, 100 revisions. DropBox allows old versions to be stored for longer but you have to pay. 
* Version control like Git is only bounded by space available.

Use tags as nicknames for commit identifiers
--------------------------------------------

    git tag BOOT_CAMP  # Human-readable name for cryptic commit identifier.
    git tag

Make and commit changes to `recipe.md`.

    git checkout BOOT_CAMP
    git checkout master

Tags:

* Tag when scripts or code released e.g. `VERSION.1.2` so can retrieve these versions to fix bugs.
* Tag when configuration files, scripts and code are used to generate data for a paper e.g. `JPHYSCOMP.03.14` so can redo an analysis if paper comes back from reviewers with questions, or reader can recreate analyses.
* Use easy-to-remember, meaningful, self-documenting names, as for variable, function, class and script names.

Branches
--------

    git status recipe.md

* `master` is a branch - a set of related commits made to files the repository, each of which can be used and edited and updated concurrently. 
* `master` is Git's default branch.
* Create a new branch from any commit at any time. 
* When complete, merge the branch into another branch, or into `master`.

Question: why might this be useful?

Answer:

* We release code and scripts to users or other researchers. We continue developing. User finds a bug. We can tell them to wait till we've done our next version, release a half-complete version, release a patch or use a branch from our last release and release a bug-fixed version.
* Experiment with developing a new feature, or an optimisation, we're not sure we'll keep.
* Simultaneously prepare a paper for submission and add a new section for a future submission.

Pretty-print log:

    git log --oneline --graph --decorate --all

View and create branches:

    git branch  # See all branch names. * is current branch.
    git branch servings
    git branch
    git checkout servings
    git branch  # Two concurrent branches now exist.

In `servings`, make and commit changes to the end of `recipe.md`, pretty-print log.

Checkout `master`, make and commit changes to existing lines of `recipe.md`, pretty-print log.

Checkout `servings`, make and commit changes to the end of `recipe.md`, pretty-print log.

Checkout `master`, make and commit changes to existing lines of `recipe.md`, pretty-print log.

    git merge servings  # Merge changes from servings into master

Merging is done file-by-file, line by line. A new commit is automatically created to represent the merge.

    git log
    git log --oneline --graph --decorate --all

What happens if we edit the same lines in both branches?

Checkout `servings`, make and commit changes to `recipe.md`, pretty-print log.

Checkout `master`, make and commit changes to the same lines of `recipe.md`, pretty-print log.

    git merge servings

`CONFLICT` - changes can't be seamlessly merged because changes have been made to the same set of lines in the same files.

    git status

`Unmerged` - files which have conflicts.

    cat recipe.md

Conflict markup:

* `<<<<<<< HEAD` - conflicting lines local commit.
* `=======` - divider between conflicting regions.
* `>>>>>>> 71d34decd32124ea809e50cfbb7da8e3e354ac26` - conflicting lines from remote commit.

Conflict resolution - edit and do one of:

* Keep the local version, which, here, is the one marked-up by `HEAD`.
* Keep the other version, which, here, is the one marked-up by the commit identifier.
* Or keep a combination of the two.

Resolve conflict:

    git add recipe.md  # Explicit add and commit to resolve conflict.
    git commit -m "Resolved confict in recipe.md by ..."
    git log
    git log --oneline --graph --decorate --all

Delete branch when no longer needed.

    git branch -D servings

Review using [Images of the key steps in this section](GitBranches.md)

DropBox and GoogleDrive don't support this ability. 

No work is ever lost.

Summary
-------

* Keep track of changes like a lab notebook for code and documents.
* Roll back or forward, undo and redo, changes to any point in the history of changes.
* Use branches to work on concurrent versions of the repository.

Question: what problems or challenges do we still face in managing our files?

Answer:

* If we delete our repository not only have we lost our files we've lost all our changes!
* Suppose we're away from our usual computer, for example we've taken our laptop to a conference and are far from our workstation, how do we get access to our repository then?

Work from multiple locations with a remote repository
=====================================================

Repository hosting:

* Site or institution-specific repositories.
* [GitHub](http://github.com) - pricing plans to host private repositories.
* [Bitbucket](https://bitbucket.org) - free, private repositories to researchers. 
* [Launchpad](https://launchpad.net) 
* [GoogleCode](http://code.google.com)
* [SourceForge](http://sourceforge.net)

Version control plus integrated tools e.g. visualise branches, browse histories, automatic e-mails with commits, syntax highlighting, visualising diffs, release management, wikis, issue trackers, user management etc.

Get an account
--------------

* [Sign-up for free GitHub account](https://github.com/signup/free)
* [Sign-up for free BitBucket account](https://bitbucket.org/account/signup/)

Create new repository
---------------------

GitHub:

* Log in to [GitHub](https://github.com)
* Click on the Create a new repo icon on the top right, next to your user name
* Enter Repository name: `cookbook`
* Make sure the Public option is selected
* Make sure the Initialize this repository with a README is unselected
* Click Create Repository

BitBucket:

* Log in to [Bitbucket](https://bitbucket.com).
* Click on Create icon on top left, next to Bitbucket logo.
* Enter Repository name: `cookbook`.
* Check private repository option is ticked.
* Check repository type is `Git`.
* Check Initialize this repository with a README is unselected.
* Click Create Repository.

Question: is publicly visible code on BitBucket or GitHub open source?

Answer: yes, but only if it has an open source licence. Otherwise, by default, it is "all rights reserved".

'Push' `master` branch to GitHub:

    git remote add origin https://github.com/USERNAME/cookbook.git  # Alias for repository URL.
    git push -u origin master  # Set local repository to track remote repository.

'Push' `master` branch to BitBucket:

    git remote add origin https://USERNAME@bitbucket.org/USERNAME/cookbook.git  # Alias for repository URL.
    git push -u origin master   # Set local repository to track remote repository.

Check master branch on GitHub:

* Click Code tab.
* Click Commits tab.
* Click Network tab.

Check master branch on BitBucket:

* Click Source tab.
* Click Commits tab.

Clone remote repository
-----------------------

Something dramatic and dire happens:

    rm -rf cookerybook

Copy, or 'clone', the remote repository:

    git clone https://github.com/USERNAME/cookbook.git

    git clone https://USERNAME@bitbucket.org/USERNAME/cookbook.git

    cd cookbook
    git log
    ls -A

Question: where is the `cookerybook` directory?

Answer: `cookerybook` was the directory that held our local repository but was not a part of it.

Push changes to remote repository
---------------------------------

Make and commit changes to `recipe.md`.

    git push

Refresh web pages and check that changes are now in the remote repository.

Collaboration
=============

Form into pairs and swap GitHub / BitBucket user names.

One of you share your repository with your partner - we'll call you the Owner:

* GitHub - Owner click on the Settings tab, click on Collaborators, and add partner's GitHub name.
* BitBucket - Owner click on the Share link, and add partner's BitBucket name.

Both Owner and partner clone the Owner's repository e.g.

    git clone https://github.com/OWNERUSERNAME/cookbook.git
  
    git clone https://USERNAME@bitbucket.org/USERNAME/cookbook.git 

Owner make, commit and push changes to `recipe.md`.

Pull changes from a remote repository
-------------------------------------

Partner 'fetch' changes from remote repository:

    git fetch
    git diff origin/master

`diff` compares current, `master` branch, with `origin/master` branch. This is the name of the `master` branch in `origin` and is the alias for the cloned repository, the one in our remote repository.

'Merge' changes into current repository, which merges the branches together:

    git merge origin/master
    cat recipe.md

Partner make, commit and push changes to `recipe.md`.

Owner 'fetch' changes from remote repository.

Partner 'pull' changes from remote repository:

    git pull
    cat recipe.md
    git log

`pull` does a `fetch` and a `merge` in one go. 

Exercise - collaborate
======================

Owner and partner alternatively pull, change, commit, push.

Owner and partner together:

* Both edit different lines of the same file, add it and commit it.
* Both push.
* Slower one pull then push.
* Both edit same lines of the same file, add it and commit it.
* Both push.
* Slower one pull, resolve conflicts, push.
* Repeat! 
* Try editing and adding new files and directories too.

Summary
-------

* Host repository remotely.
* Copy, or clone, remote repository onto local machine
* Make changes to local repository and push these to remote repository
* Fetch and merge, or pull, changes from remote repository into local repository
* Identify and resolve conflicts when the same file is edited within two repositories

Exercise - Copy the boot camp material
======================================

* Create a `bootcamp` repository on GitHub/BitBucket.
* Change into the directory you cloned at the start of the boot camp.
* Push this repository to your remote `bootcamp` repository.
* Keep using this repository throughout the rest of the boot camp!

Conclusion
==========

* Provenance:
 * Record changes like a lab notebook for scripts, files, code and documents - ideas explored, fixes made, refactorings done, false paths explored - what was changed, who by, when and why.
 * Roll back and foward, undo and redo, changes to any point in the history of changes to files and directories.
 * Bind results, data and papers to scripts, code, configuration data that produced them - Git hashes, commit identifiers and tags.
* Experiment:
 * Confidence to play around, experiment without cryptic naming schemes or risking losing your work.
* Back-up:
 * Back up entire history of changes in various locations.
* Collaborate - with others and with yourself:
 * Work on files from multiple locations.
 * Identify and resolve conflicts when the same file is edited within two repositories without losing any work.
 * Collaboratively work on code or documents or any other files without any loss of work and with full provenance and accountability.
 * Find out what was changed, who by, when and why.
 * Version control is just as useful, relevant, powerful, helpful, necessary for a solo researcher as for a team of researchers.

"If you are not using version control then, whatever else you may be doing with a computer, you are not doing science" -- Greg Wilson
