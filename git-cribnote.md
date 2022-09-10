# Working with local git repository

## Initialize local git repository.

Turn a project/working directory into a git repository. This will create a (`.git`) a (hidden) sub-folder inside.
the project folder

```markdown
git init
```

## Checking present git status.

Show current status of git repository.

```markdown
git status

> for example;
$ git status
On branch master
nothing to commit, working tree clean
$
```

## Recording changes in project folder to staging area.

Bring changes made in project folder to staging area before we actually create a commit in repository.

```markdown
> add an specific file to staging area;
git add <filename>
> add all changes made to project folder to staging area;
git add .
```

<img src="lifecycle.png"  width="400" height="200">

## Create a commit in git repository.

Commits in a repository are stages/milestones created/marked. Each commit has a unique ID.
We always have to bring changes to staging area before we commit.
It is a good practice to use (`git add .`) before using following command.

```markdown
git commit -m (--message) "Message to associate with a commit"
```

## Diving deeper into commits with git log.

Each commit in repository will create a log. We can inspect all commits in the branch using;

```markdown
git log

> for example;
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

```markdown
> to checkout any previous commit;
git checkout <ID>
> to return back to project present state;
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

## Understanding the HEAD and detached HEAD

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
