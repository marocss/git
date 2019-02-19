# GIT

> ˜= 15 min. 

How to use GIT 👌


## Basic workflow

### What is version control?
- a version control system manages changes in files and directories in a project.
- Git Strenghts:
	1. nothing is ever lost. Check different versions of your program.
	2. notify work conflics.
	3. sincronize work done by different people on different machines.

### Where does GIT store information?
- repository: combination of files and directories and extra info Git records about the project.
	extra info is stored at a ".git" directory in the root directory of your repository.

### How can I check the state of a repository?
- `git status` : displays list of files that changes have been made after previous save.

### How can I tell what I have changed?
- staging area: where git stores files with changes you want to save that haven't been saved yet.
- `git status` shows files that are in this staging area, and files that have changes that haven't yet been put there.
- while files are at the staging area you can add and remove things, after you committed you can't make no further changes.
- `git diff <filename>` : compare the file as it currently is to what you last saved.
- `git diff` : all changes in the repository.
- `git diff <directory>` : all changes to the files in the directory.

### What is in a diff?
- diff is a formatted display of the difference between 2 sets of files.
	- ex:
		```diff --git a/report.txt b/report.txt
		index e713b17..4c0742a 100644
		--- a/report.txt
		+++ b/report.txt
		@@ -1,4 +1,4 @@
		-# Seasonal Dental Surgeries 2017-18
		+# Seasonal Dental Surgeries (2017) 2017-18

		    TODO: write executive summary.
		```
	- command used to produce output: diff --git.
	- a / b are placeholders meaning first version and second .
	- index line showing keys into internal Git's internal database of changes.
	- `--- a/re...` `+++ b/re...` : lines being removed and lines being added.
	- `@@` : line starting with these tell where the chages are beeing made. Pairs of number are <start line>, <num of lines changed>.
	- line-by-line list of changes. `-` shows delitions, `+` shows additions. Lines without change may appear for context without `+` or `-`.
- RStudio - turns diffs into readable side-by-side display of changes.
- Standalone tools: DiffMerge or WinMerge.

### What's the first step in saving changes?
- commit changes to a git repository in 2 steps:
	1. add file(s) to staging area.
          `git add <filename>` : adds to staging area.
		after in the staging area `git diff <file>` won't work.
		use `git diff -r HEAD`.
	2. commit everything from staging area.

### How can I tell what's going to be committed?
- `git diff -r HEAD` : compare the state of your files to the files in the staging area.
	- `-r` flag means "compare to a particular revision".
	- `HEAD` is a shortcut meaning "the most recent commit".
- `git diff -r HEAD path/to/file` : restrict results to single file or directory, where the path is relative to where you are.
- `git diff HEAD` : same as with `-r` flag. Compares current working directory with latest commit.
- `git diff --cached` : compares current staging area with previous commit.
	- `git diff --staged` is the same.

Interlude: how can I edit a file?
- nano : Unix text editor.
- `nano <filename>` : opens file or creates it.
- use arrow keys to move around and backspace.
- other commands with ctrl:
	- ctrl + K : delete line.
	- ctrl + U : un-delete line.
	- ctrl + O : save file (O - "output").
	- ctrl + X : exit editor.
	- ctrl + space : move one word at a time.

### How do I commit changes?
- `git commit` : save changes from staging area. It saves everything in the staging area as one unit.
- when you want to undo changes to a project you undo all of a commit or none of it.
- log message: required by Git when you want to commit. same purpose as a comment in a program.
- Git launches a text editor by default on commit to write your log message.
	or use: `git commit -m "Program appears to have become self-aware."`
- `git commit --amend -m "new message"` : use when you accidentally mistype a commit message.

### How can I view a repository's history?
- `git log` : view log of the project's history. Are shown most recent first.
	- ex:
		```commit 0430705487381195993bac9c21512ccfb511056d
		Author: Rep Loop <repl@datacamp.com>
		Date:   Wed Sep 20 13:42:26 2017 +0000

		    Added year to report title.
		```
	- commit line displays unique ID to commit called hash.
	- who made the change.
	- log message.
- git log automatically uses a pager to show one screen of output at a time. Use spacebar to go down a page or "q" to quit.

### How can I view a specific file's history?
- `git log <path>`
	- if path == file: shows changes made to that file.
	- if path == directory: shows changes made to files in that directory.

### How do I write a better log message?
- use only `git commit` so it opens an editor for multi-line comment.



## Repositories

### How does Git store information?
- Git uses a three-level structure to store the information of each commit that you make.  1. metadata : author, commit message, time of commit.
	2. tree : tracks the names and locations in the repository when that commit happend.
	3. blob (binary large object) : a SQL database term for "may contain data of any kind". Contains a compressed snapshot of the contents of the file when the commit happend. If a file does not change in a commit it maintains the same blob value from the previous commit.

### What is a hash? 
- unique identifier for every commit.
- generated by a hash function.
- if two files are the same, their hashes are the same.
- if 2 commits have the same files and same ancestors, their hashes will also be the same.
- git tells what information needs to be saved where by comparing hashes rather than files.

### How can I view a specific commit?
- `git show <first characters of hash>`.
	- ex:
		```commit 0da2f7ad11664ca9ed933c1ccd1f3cd24d481e42
		Author: Rep Loop <repl@datacamp.com>
		Date:   Wed Sep 5 15:39:18 2018 +0000

		    Added year to report title.

		diff --git a/report.txt b/report.txt
		index e713b17..4c0742a 100644
		--- a/report.txt
		+++ b/report.txt	
		@@ -1,4 +1,4 @@
		-# Seasonal Dental Surgeries 2017-18
		+# Seasonal Dental Surgeries (2017) 2017-18

		    TODO: write executive summary.
		```
	  - first part is the same as the git log.
	  - second part shows the changes, like git diff.

### What is Git's equivalent of a relative path?
- hash == absolute path.
- `HEAD` : relative path. Refers to most recent commit.
- `HEAD~1` : refers to the commit before that.
- `HEAD~2`

### How can I see who changed what in a file?
- `git annotate <file>` : shows who made the last change to each line of a file and when.
	- ex:
		```04307054        (  Rep Loop     2017-09-20 13:42:26 +0000       1)# Seasonal Dental Surgeries (2017) 2017-18
		5e6f92b6        (  Rep Loop     2017-09-20 13:42:26 +0000       2)
		5e6f92b6        (  Rep Loop     2017-09-20 13:42:26 +0000       3)TODO: write executive summary.
		```
	1. the first 8 digits of the hash.
	2. the author, Rep Loop.
	3. the time of the commit.
	4. the line number.
	5. contents of the line.

### How can I see what changed between two commits?
- `git show <commit ID>` : shows changes made in a particular commit.
- `git diff ID1..ID2` : shows changes between 2 commits. ID's identifies the 2 commits you are interested. The ".." is the connector.
- `git diff HEAD~1..HEAD~3` : using relative path.

### How do I add new files?
- git waits until the first `git add` to start paying attention to a file.
- untracked files won't have a blob and won't be listed in a tree.
- `git status` will tell about files that are in the repository but aren't being tracked.

### How do I tell Git to ignore certain files?
- create a file at the root of the repository called `.gitignore`.
- store in that file a list of wildcard patterns that specify the files you don't want Git to pay attention to.
	- ex: (`.gitignore` file contents)
		```
		build
		*.mpl
		```
	- git will ignore any file or directory called `build` (and anything inside the directory).
	- will ignore any file ending with `.mpl`.

### How can I remove unwanted files? 
- Git can clean up files you told it you don't want.
- `git clean -n` : shows list of files that are in the repository but whose history git is not currently tracking.
- `git clean -f` : will delete those files.
- WARNING: git clean only works on untracked files, their history haven't been saved. Once deleted they are GONE.

### How can I see how Git is configured?
- `git config --list` : use to see what the settings are with one of the 3 additional options.
 `--system` : settings for every user on this computer.
 `--global` : settings for every one of your projects.
 `--local` : settings for one specific project.
- each level overrides the one above it.
	- Config settings are useful for storing your name and email address (to identify you in commit logs), choosing your favorite text editor and diff view tools, and customizing things just how you like them.

### How can I change my Git configuration?
- they should be unchaged. but always alter your name and email.
- to change a config value for all your projects on a computer run:
	- `git config --global setting.name setting.value`
- the keys that identify your name and email address are `user.name` and `user.email`.
	- ex: 
		```git config --global user.email email@email.com```



## Undo

### How can I commit changes selectively?
- you don't have to put all recent changes in the staging area at once.
- `git add path/to/file` : stage a single file.
- `git reset HEAD` : unstage additions in case of mistankenly staging a file.

### How do I re-stage files?
- use `git add` to periodically save most recent changes. Useful when the changes are experimental and you might want to undo them without cluttering up the repository's history.

### How can I undo changes to unstaged files?
- `git checkout -- filename` : discards changes not yet staged. `--` separates the git checkout command from the name(s) of the file(s) you want to recover.
- WARNING : once changes are discarted this way they are gone forever.

### How can I undo changes to staged files?
- by combining `git reset` with `git checkout` you undo changes to a staged file.
	- ex:
		```
		git reset HEAD path/file
		git checkout -- path/file
		```
	- (It uses 2 commands because unstaging a file and undoing changes are special cases of more powerfull git operations that you have yet to learn).

### How do I restore an old version of a file?
- `git checkout` : can also be used to go back into a file's history and restore versions of that file from a commit.
- think of commiting as saving your work and "checking out" as loading that saved version.
- to restore old file use 2 arguments, hash of the version you want and the filename.
	- ex: (`git log`)
		```
		commit ab8883e8a6bfa873d44616a0f356125dbaccd9ea
		Author: Author: Rep Loop <repl@datacamp.com>
		Date:   Thu Oct 19 09:37:48 2017 -0400

		    Adding graph to show latest quarterly results.

		commit 2242bd761bbeafb9fc82e33aa5dad966adfe5409
		Author: Author: Rep Loop <repl@datacamp.com>
		Date:   Thu Oct 16 09:17:37 2017 -0400

		    Modifying the bibliography format.
		```
	- `git checkout 2242bd report.txt` : will replace current `report.txt` with committed version on oct 16.
	- same syntax to undo unstaged file, but `--` is replaced by hash.
- does not delete any of the repository's history. It is saved as another commit, in case case you might want to undo your undoing.
- `git log -3 report.txt` : shows last 3 commits involving the file `report.txt`.

### How can I undo all of the changes I have made?
- `git reset HEAD <directory>` : unstages any files inside directory.
- `git reset HEAD` : unstages everything if you don't provide any file(s) or directory(ies).
- `git reset` : `HEAD` is the default commit to unstage, so you can simply write `git reset` to unstage everything.
- `git checkout -- <directory>` : will restore the files in directory to their previous state.
- `git checkout -- .` : will revert all files in the current directory.



## Working with branches

### What is a branch?
- branches : allows for multiple versions of the same work and lets you track of each version systematically.
- changes you make in one branch do not affect others. (until they are merged).
- Git data structure used to record a repository's history:
	1. blob for files.
	2. trees for saved states of the repositories.
	3. commits to record the changes.
- branches are why Git needs trees and commits: a commit will have two parents when brances are being merged.

### How can I see what branches my repository has?
- `master` : default branch for every Git repository.
- `git branch` : lists all branches in a repository.
	- the branch you are currently in is shown with a `*`.

### How can I view the differences between brances?
- `git diff branch-1..branch-2` : shows the difference between 2 brances.
	- like `git diff revision-1..revision-2` shows the difference between 2 versions of a repository.

### How can I switch from one branch to another?
- `git checkout` : can also be used to switch to a branch.
	- like `git checkout hash` to switch the repository state to that hash.
- Two notes:
	1. `git branch` puts a `*` beside the name of the current branch you are in.
	2. Git only lets you do this if all changes have been commited.
		- (there's a way to do this without commiting but it's hard tho?!).
- `git rm` : removes the file then stages the removal of that file with `git add`, all in one step.

### How can I create a branch?
- `git branch branch-name` : creates a branch.
	- but it's more common to create a branch and move to that branch.
- `git checkout -b branch-name` : creates and switches to it in one step.
- contents of new branch are identical to contents of the original. Once you start the changes, they will only affect the new branch.
- `git diff master..new-branch` : compares branches.

### How can I merge two branches?
- when you merge a branch (source) into another (destination), Git incorporates the changes made to the source into the destination branch. If there is no overlap, the result is a new commit in the destination branch that includes everything from the source.
- `git merge source destination` : Git opens an editor to write a log message for the merge. Keep it as default or fill something informative.

### What are conflicts?
- bug fixes might touch the same lines of code.
- Git relies on you to reconcile the conflicting changes.

### How can I merge two branches with conflicts?
- run `git status` after the merge conflict to remind which files have conflicts that you need to resolve by printing `both modified:` beside the files' names.
- markers left by Git to tell you where the conflicts occured:
	```
	<<<<<<< destination-branc-name
	...changes from the destination branch...
	======
	...changes from the source branch...  >>>>>>> source-branc-name
	```
- in many cases the destination branch name will be `HEAD`, because you will be merging into the current branch.
- to resolve edit the file removing the markers and making changes where they are needed to reconcile the changes, then add to staging area and  commit those changes.



## Collaborating

### How can I create a brand new repository?
- `git init project-name` : creates a repository for a new project. "project-name" is the name you want the repository's root directory to have.
- don't create one Git repository inside another. Git allows but nested repositories can become complicated, since you need to tell Git whitch of the 2 `.git` dirs the update is to be stored in.

### How can I turn an existing project into a Git repository?
- `git init` : run in the project's root directory.
- `git init /path/to/proj` : or run this from anywhere on your computer.
- after initializing a folder into a repository, Git notices that there are a bunch of changes that can be staged (and committed later).

### How can I create a copy of an existing repository?  
- sometimes you will: join a project that is already running; inherit a project from someone; continue working on one of your own on a new machine. In these cases you will clone a repository instead of creating a new one.
	- (copies a repo including all of its history into a new dir)
- `git clone URL` : URL identifies the repository you want to clone.
	- (https://github.com/datacamp/project.git)
- `git clone /existing/project` : git uses the name of the existing repository as the name of the clone's root directory.
	- ex:
		```	
		git clone /existing/project
		```
	- creates a new directory called `project`.
- `git clone /existing/project newprojectname` : use to call the clone something else.

### How can I find out where a cloned repository originated?
- remote : like a browser bookmark with a name and a URL. Git uses it by storing in the new repository's configuration.
- if you clone from GitHub, the copy on the website is the remote.
- `git remote` : lists the names of its remotes.
- `git remote -v` : ("verbose") if you want more information this shows the remote's URLs.

### How can I define remotes?
- when you clone a repository Git automatically creates a remote called origin that points to the original repository.
- `git remote add remote-name URL` : add more remotes.
- `git remove rm remote-nme` : remove existing remotes.
- you can connect any 2 Git repositories this way, but in practice, you will almost always connect repositories that share some common ancestry.

### How can I pull in changes from a remote repository? 
- Git keeps track of remote repositorys so that you can pull and push changes to them.
- typical workflow: pull in collaborators' work from remote repository, do some work, push your work.
- `git pull remote branch` : gets everything in `branch` in the remote repository identified by `remote` and merges it into the current branc of your local repository.
	- ex: (if you are in the `quarterly-report` branch of your local repository)
		- `git pull thunk latest-analysis`
	- gets changes from latest-analysis branch in the repository associated with the remote called thunk and merge them into your quarterly-report branch.  - `git pull remote` : gets everything from that repository.

### What happens if I try to pull when I have unsaved changes?
- Git stops you from pulling in changes from a remote repository when doing so might overwrite thing you have done locallu.
- either commit your local changes r revert them and try to pull again.
- `git checkout -- <file>` : use to revert changes in a file.

### How can I push my changes to a remote repository?
- `git push` pushes the changes you made locally into a remote repository.
- `git push remote-name branch-name` : pushes the contents of your branch `branch-name` into a branche with the same name in the remote repository associated with `remote-name`.
	- it's possible to use different branch names at your end and the remotes end but it can become confusing. Almost always it is betther to use the same names for branches across repositories.

### What happens if my push conflicts with someone else's work?
- Git does not allow you to push changes to a remote unless you have merged the contents of the remote repository into your own work.
- use `git pull` to bring your repository up to date.
- `git pull origin master --no-edit` : to pull and avoid the editor from opening.
