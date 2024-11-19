###### Creating a new local branch to track existing remote branch
```bash
git branch --track branch-name origin/branch-name
```
###### Undoing latest commit ( not pushed )
[source](https://stackoverflow.com/questions/927358/how-do-i-undo-the-most-recent-local-commits-in-git)
**Option 1** 
You want to **destroy commit C and also throw away any uncommitted changes**. You do this:
```bash
git reset --hard HEAD~1
```

 **Option 2**
 Maybe commit C wasn't a disaster, but just a bit off. You want to **undo the commit but keep your changes** for a bit of editing before you do a better commit. 
 ```bash
 git reset HEAD~1
```

**Option 3**
For the lightest touch, you can even **undo your commit but leave your files staged for commit.**
```bash
git reset --soft HEAD~1
```

**Getting code back after `git reset --hard`**
 Type this

```bash
git reflog
```

and you'll see a list of (partial) commit SHAs (that is, hashes) that you've moved around in. Find the commit you destroyed, and do this:

```bash
git checkout -b someNewBranchName shaYouDestroyed
```

###### Undoing latest commit ( pushed )
if the commit was already pushed, do this:
```bash
git revert <bad-commit-sha1-id>
```
then:
```bash
git push origin
```

###### Switching to a previous commit
Get the list of previous commits along with their commit id
```bash
git log --oneline
```

**Switch into Detached Head State**
- This detaches the HEAD from the current branch and points it directly to the specified commit.
- Any changes you make won't be reflected in any existing branch. You'll need to explicitly create a new branch or commit your changes to a new commit object not associated with any branch.
- This is useful for inspecting the state of a specific commit in your history or cherry-picking individual commits.
```bash
git checkout <commit-id>
```

**Switching to Make Changes**
- This command checks out only the contents of the current directory (and its subdirectories) from the specified commit.
- It does not move the HEAD or change your current branch.
- Only the files in the current directory and below will be updated to match the state of the specified commit.
- Your HEAD remains where it was, typically on the current branch.
```bash
git checkout <commit-id> .
```

###### Switching back to latest commit ( if no changes made )
```bash
git checkout <branch-name>
```
This will move the HEAD to latest commit in the branch