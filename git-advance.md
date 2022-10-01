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
commit 1711b0f9dbd240ccd359e8aaf4638c74331125c6 (HEAD -> master)
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
commit cb34841215e6362df73fcaef5d2fabfee5f05526 (HEAD -> master)
Author: TABISH <email@example.com>
Date:   Fri Sep 9 23:15:28 2022 +0530

    third commit

commit 62caabb1bf20f2864fcfdbd15d3c0b480ba4179c
Author: TABISH <email@example.com>
Date:   Fri Sep 9 23:13:13 2022 +0530

    another commit
$
```
