[Learn git's basics](/git-cribnote.md)

# Git Advanced Topics

## Understanding The Stash

**Stash is kind of git's internal memory where we can save uncommitted and un-staged changes.**

Use git stash when you want to record the current state of the working directory and the index, but want to go back to a clean working directory. The command saves your local modifications away and reverts the working directory to match the HEAD commit.

The modifications stashed away by this command can be listed with git stash list, inspected with git stash show, and restored (potentially on top of a different commit) with git stash apply. Calling git stash without any arguments is equivalent to git stash push. A stash is by default listed as "WIP on branchname ...", but you can give a more descriptive message on the command line when you create one.

> **_Always use command_** `git stash push -m "<message>"` **_so that this modification to the project can be recalled seeing comment in the stash list._**

```markdown
$ git stash push -m "This is a good improvement"
Saved working directory and index state On master: This is a good improvement
$ git stash list
stash@{0}: On master: This is a good improvement
stash@{1}: WIP on master: cb34841 third commit
stash@{2}: WIP on master: cb34841 third commit
stash@{3}: WIP on master: cb34841 third commit
stash@{4}: WIP on master: cb34841 third commit
stash@{5}: WIP on master: cb34841 third commit
$

$ git stash pop 0 <- Brings back modification and drop/delete that stash
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   testfile

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (6785a35af01bdace20a3bbb0b4b6eed93a2f7ea7)
```

> `git stash apply <index ID>` **_doesn't delete/drop the stash._**

```markdown
$ git stash list
stash@{0}: WIP on master: cb34841 third commit
stash@{1}: WIP on master: cb34841 third commit
stash@{2}: WIP on master: cb34841 third commit
stash@{3}: WIP on master: cb34841 third commit
stash@{4}: WIP on master: cb34841 third commit
$
```

> Use `git stash drop <index ID>` to delete specific stash OR use; `git stash clear` for deleting all stashes.

```markdown
$ git stash drop 0
$
$ git stash list
stash@{0}: WIP on master: cb34841 third commit
stash@{1}: WIP on master: cb34841 third commit
stash@{2}: WIP on master: cb34841 third commit
stash@{3}: WIP on master: cb34841 third commit
$ 
$ git stash clear 
$ git stash list
$
```

## Bringing Lost Data Back 

With `"git reflog"` we can restore deleted commit back in project. To show how this works first we have to delete a commit OR reset git back to a previous commit. Let's create a new commit in project and discard it using `git reset --hard <commit ID>`

```markdown
$ git commit -m "latest commit"
[master 1711b0f] latest commit
 1 file changed, 3 insertions(+)
$
```
```markdown
$ git log
> commit 1711b0f9dbd240ccd359e8aaf4638c74331125c6 (HEAD -> master)
Author: TABISH <email@example.com>
Date:   Sat Oct 1 11:38:58 2022 +0530

    latest commit

commit cb34841215e6362df73fcaef5d2fabfee5f05526
Author: TABISH <email@example.com>
Date:   Fri Sep 9 23:15:28 2022 +0530

    third commit

commit 62caabb1bf20f2864fcfdbd15d3c0b480ba4179c
Author: TABISH <email@example.com>
Date:   Fri Sep 9 23:13:13 2022 +0530

    another commit
$
```
```markdown
$ git reset --hard cb34841215e6362df73fcaef5d2fabfee5f05526
HEAD is now at cb34841 third commit
$
```
```markdown
$ git log
> commit cb34841215e6362df73fcaef5d2fabfee5f05526 (HEAD -> master)
Author: TABISH <email@example.com>
Date:   Fri Sep 9 23:15:28 2022 +0530

    third commit

commit 62caabb1bf20f2864fcfdbd15d3c0b480ba4179c
Author: TABISH <email@example.com>
Date:   Fri Sep 9 23:13:13 2022 +0530

    another commit
$
```
> **_Git keeps all record/changes in repository for last 30 days._**

> Run `git reflog` to check all those changes.

```markdown
$ git reflog 
cb34841 (HEAD -> master) HEAD@{0}: reset: moving to cb34841215e6362df73fcaef5d2fabfee5f05526
> 1711b0f HEAD@{1}: commit: latest commit
cb34841 (HEAD -> master) HEAD@{2}: reset: moving to HEAD
cb34841 (HEAD -> master) HEAD@{3}: reset: moving to HEAD
cb34841 (HEAD -> master) HEAD@{4}: reset: moving to HEAD
cb34841 (HEAD -> master) HEAD@{5}: reset: moving to HEAD
cb34841 (HEAD -> master) HEAD@{6}: reset: moving to HEAD
cb34841 (HEAD -> master) HEAD@{7}: reset: moving to HEAD
cb34841 (HEAD -> master) HEAD@{8}: checkout: moving from 62caabb1bf20f2864fcfdbd15d3c0b480ba4179c to master
62caabb HEAD@{9}: checkout: moving from master to 62caabb1bf20f2864fcfdbd15d3c0b480ba4179c
cb34841 (HEAD -> master) HEAD@{10}: commit: third commit
62caabb HEAD@{11}: commit: another commit
3f8fa38 HEAD@{12}: commit (initial): initial commit
$
```
> Copy the lost commit's ID from the list and use as below;

```markdown
$ git reset --hard 1711b0f
HEAD is now at 1711b0f latest commit
```
> Now we have the lost commit back in the project. Check using `git log`;

```markdown
$ git log
> commit 1711b0f9dbd240ccd359e8aaf4638c74331125c6 (HEAD -> master)
Author: TABISH <email@example.com>
Date:   Sat Oct 1 11:38:58 2022 +0530

    latest commit

commit cb34841215e6362df73fcaef5d2fabfee5f05526
Author: TABISH <email@example.com>
Date:   Fri Sep 9 23:15:28 2022 +0530

    third commit

commit 62caabb1bf20f2864fcfdbd15d3c0b480ba4179c
Author: TABISH <email@example.com>
Date:   Fri Sep 9 23:13:13 2022 +0530

    another commit
```
> **`git reflog` _also helps us with deleted branch._** Let's create a new branch to check branch restore.

> Create a "feature" branch and switch to it immediately
```markdown
$ git checkout -b feature
Switched to a new branch 'feature'
$
```

> List branches
```markdown
$ git branch 
* feature
  master
$
```

> Add a new file to the project and commit it.
```markdown
$ git ls-files
anotherfile
testfile
```

> Create a new file and add this to staging area
```markdown
$ touch feature-file
$ git add .
```

> Check files in staging area
```markdown
$ git ls-files
anotherfile
feature-file
testfile
```

> Commit this change (in feature branch)
```markdown
$ git commit -m "file added in feature"
[feature 944d9f0] file added in feature
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 feature-file
$
```

> Switch back to master branch
```markdown
$ git switch master 
Switched to branch 'master'
```

> Delete feature branch
```markdown
$ git branch -D feature 
Deleted branch feature (was 944d9f0).
$ git branch
* master
$
```

> Check reflog
```markdown
$ git reflog 
1711b0f (HEAD -> master) HEAD@{0}: checkout: moving from feature to master
944d9f0 HEAD@{1}: commit: file added in feature
1711b0f (HEAD -> master) HEAD@{2}: checkout: moving from master to feature
1711b0f (HEAD -> master) HEAD@{3}: reset: moving to 1711b0f
cb34841 HEAD@{4}: reset: moving to cb34841215e6362df73fcaef5d2fabfee5f05526
1711b0f (HEAD -> master) HEAD@{5}: commit: latest commit
cb34841 HEAD@{6}: reset: moving to HEAD
cb34841 HEAD@{7}: reset: moving to HEAD
cb34841 HEAD@{8}: reset: moving to HEAD
cb34841 HEAD@{9}: reset: moving to HEAD
cb34841 HEAD@{10}: reset: moving to HEAD
cb34841 HEAD@{11}: reset: moving to HEAD
cb34841 HEAD@{12}: checkout: moving from 62caabb1bf20f2864fcfdbd15d3c0b480ba4179c to master
62caabb HEAD@{13}: checkout: moving from master to 62caabb1bf20f2864fcfdbd15d3c0b480ba4179c
cb34841 HEAD@{14}: commit: third commit
62caabb HEAD@{15}: commit: another commit
3f8fa38 HEAD@{16}: commit (initial): initial commit
$
```

> Checkout to the commit ID of the feature branch, this will result in detached head state.
```markdown
$ git checkout 944d9f0
Note: switching to '944d9f0'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 944d9f0 file added in feature
```

> Finally re-create feature branch having this commit.
```markdown
$ git switch -c feature
Switched to a new branch 'feature'
$
```

> List branches
```markdown
$ git branch 
* feature
  master
```
## Combining Branches (Understanding Merge Types)

There are scenarios, where we need evolved feature branch to original main/master branch, in that case we merge branches. Two key merge kinds are;

### The Fast forward Merge

The fast-forward merge only works when there is no additional commits in master/main branch since feature branch was created. Merge moves HEAD forward but doesn't create new commit.

> Let's initialize a git repository and create first commit in master branch.

```markdown
> add a file in project
$ touch master.txt
> initialize git repository
$ git init
> add changes/files to git's staging area
$ git add .
> finally create a commit in master branch
$ git commit -m "M1"
[master (root-commit) 9c11a9f] M1
 1 file changed, 1 insertion(+)
 create mode 100644 master.txt
$
```
> Now we have a master branch with one commit in it, let's create a feature branch.

```markdown
> create a feature branch
$ git switch -c feature
Switched to a new branch 'feature'
$
> create a new update in feature branch
$ touch feature.txt
> add change to staging area
$ git add .
> create a commit in feature branch
$ git commit -m "f1"
[feature b708b72] f1
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 feature.txt
$
> check the log now, it shows commit in master and a commit in feature branch
$ git log
commit b708b7278f32bbf0f6e27cd1335140d4b6399b49 (HEAD -> feature)
Author: TABISH <email@example.com>
Date:   Sat Oct 8 17:43:37 2022 +0530

    f1

commit 9c11a9f113a7910cec569388c8e8015b2187f163 (master)
Author: TABISH <email@example.com>
Date:   Sat Oct 8 17:34:42 2022 +0530

    M1
$
```
> Finally implement the changes in feature branch to master branch

```markdown
> switch to master branch
$ git switch master 
Switched to branch 'master'
$
> merge with feature branch
$ git merge feature 
Updating 9c11a9f..b708b72
Fast-forward <--[Git applied the Fast-Forward Merge]
 feature.txt | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 feature.txt
$
> updates in both branches are now merged
$ git ls-files
feature.txt
master.txt
$
```