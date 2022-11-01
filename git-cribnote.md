# Working With Local Git Repository

## Initialize Local Git Repository

Turn a project/working directory into a git repository. This will create a (`.git`) a (hidden) sub-folder inside.
the project folder

```markdown
git init
```

## Checking Present Git Status

Show current status of git repository.

```markdown
$ git status
On branch master
nothing to commit, working tree clean
$
```


## Recording Changes In Project Folder To Staging Area
\
![Git's Lifecycle](./images/lifecycle.png "Git Lifecycle")

We have to bring changes made in project folder to git's staging area
before those changes are added to the commit in repository.

```markdown
git add <filename>
git add .
```

## Create A Commit In Git Repository

Commits in a repository are stages/milestones created/marked. Each commit has a unique ID.
We always have to bring changes to staging area before we commit.
It is a good practice to use (`git add .`) before using following command.

```markdown
git commit -m (--message) "Message to associate with a commit"
```

## Diving Deeper Into Commits With Git Log

Each commit in repository will create a log. We can inspect all commits in the branch using;

> for example;

```markdown
$ git log
commit cb34841215e6362df73fcaef5d2fabfee5f05526 (HEAD -> master)
Author: TABISH <name@example.com>
Date: Fri Sep 9 23:15:28 2022 +0530

    third commit

commit 62caabb1bf20f2864fcfdbd15d3c0b480ba4179c
Author: TABISH <name@example.com>
Date: Fri Sep 9 23:13:13 2022 +0530

    another commit

: <Press Q here to exit>
```
> To checkout any previous commit and to return back to project present state;

```markdown
git checkout <ID>
> return back to latest commit in the branch;
git checkout branch <existing-branch-name>
```

## Working with Branches

`"master"` - Default local git repository name. Branches are used to take full repository separate copy and work to add feature to the project in this separate branch.

```markdown
> list current branch;
git branch
> create new branch;
git branch <new-branch-name>
> switch to the newly created/any-other branch;
git checkout <new-branch-name>
> create a branch and shift to it;
git checkout -b <new-branch-name>
```

From git v2.23 onwards, we can also use `switch` option instead of checkout (the traditional way)

```markdown
> to switch to specific branch;
git switch <target-branch-name>
> to create and switch to a branch.
git switch -c (--create) <new-branch-name>
```
Branches with meaningful update or added feature to the project can be merged back with main/master branch.

```markdown
> Switch to the branch where we are merging another branch to;
git checkout/switch <main/master>
> verify that we are on the branch updates are incoming;
git status
> finally merge the branch to the existing branch;
git merge <feature-branch-name>
```

## Understanding The HEAD And Detached HEAD States

When we switch or checkout to a branch, HEAD shifts to latest commit in that branch.
If we checkout to an specific commit HEAD gets detached and moves to the checked-out commit.
Detached HEAD can be used to explore project state on an specific commit.

```markdown
> checkout specific commit using it's ID;
$ git checkout 62caabb1bf20f2864fcfdbd15d3c0b480ba4179c
Note: switching to '62caabb1bf20f2864fcfdbd15d3c0b480ba4179c'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 62caabb another commit
> verify detached head state;
$ git branch
* (HEAD detached at 62caabb)
  master
> go back to initial state (move HEAD to the branch's latest commit);
$ git checkout master
Previous HEAD position was 62caabb another commit
Switched to branch 'master'
> verify git status again;
$ git status
On branch master
nothing to commit, working tree clean
```

## Deleting File From project OR git-folder OR repository

We can delete file from project folder but that should also be deleted from git's staging area.
Either we can update staging area using (`git add .`) after deleting file(s) from project folder.
Or, we can use (`git rm <file-name>`) to remove file from staging area, before next commit.

## Undoing Un-Staged & Staged Changes

Here we explore steps involved in undoing/restoring back from un-staged/staged changes made to the repository.

> Undoing/reverting a modified file(s) which is not added in staging area.

```markdown
> undo changes in a file;
git checkout <file-name>
> undo changes in more files/sub-directories (targeting the project folder);
git checkout .
> undo using explicitly specifying current branch;
git checkout --
```
> From git v2.23 onwards we can also use (`restore`) option

```markdown
> undo changes in a file;
git restore <file-name>
> undo changes in project folder;
git restore .
```
> Get rid of **untracked file(s)** from project folder which isn't ever added to staging area)

```markdown
> list untracked file(s) to be deleted;
git clean -dn (-n or --dry-run)
> confirm the delete/clean using force option;
git clean -df (-f or --force)
```
> Traditional way of **undoing staged** changes.\
In other words, modification/files added to staging area and staging area is updated, for example; `git add .`

```markdown
git reset <file-name>
> then;
git checkout <file-name>
> after git 2.23 this is simplified using --staged flag;
git restore --staged <file-name>
> then;
git checkout <file-name>
```

## Deleting Commits

```markdown
> soft reset deletes last commit and keep changes in staging area
git reset --soft HEAD~1
> to delete latest commit and updates in staging area;
git reset --hard HEAD~1
> **We can also use commit's hash instead of using HEAD position number**
git reset HEAD <Commit ID>
```

## Deleting Branches

```markdown
> to delete a branch which is already merged;
git branch --delete <branch-name> (-d) We can specify more branches to be deleted here in same line
> to delete branch regardless the branch is merged or not;
git branch --delete --force <branch-name> (-D)
```

## Committing Changes Made In Detached-Head State

Any change we make in detached-head state, doesn't apply to the main branch. So, to bring these changes in the main branch we have to commit the change in detached-head state, copy the commit hash and create a new branch of it. Finally we can merge this new branch to the main branch.

```markdown
> get back to previous commit;
git checkout <commit ID> (this will result a detached-head situation)
> add changes to staging area;
git add . <new-file>
> create a commit with above changes;
git commit -m "message"
> shift HEAD back to original position;
git switch <branch-name>
*This result in situation loosing above commit*
> to bring this left behind commit in the branch;
git branch <new-branch> <commit ID>
git branch merge <new-branch>
> finally delete branch no longer needed
git branch -D <new-branch>
```

## The .gitignore File
.gitignore file contains file-names or wild-card entries for those file/directories, which are not required to be managed by git. This file can be placed in top of project folder or in the folder where .git directory exists.

.gitignore --- Example;

|Entry Types    |Description
|-|-|
|`.venv`        |For python virtual environment
|`*.log`        |Ignore all log files
|`!test.log`    |This will not ignore test.log file (include in staging area)
|`my-file`      |Ignore Specific file
|`web-files/*`  |This will ignore web-files directory and all it's content

### _To advance on Git's local repository, follow;_
[Git Advanced](./git-advance.md)
