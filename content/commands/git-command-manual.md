# Common git commands

## Delete a local branch

```bash
# Delete branch locally
git branch --delete <local-branch-name>

# Delete branch both remote and local branch
git push origin --delete <remote-branch-name>
```

## `git push`

Push commits to remote repository

```
git push origin(remote name) main(branch name)
```

## `git restore`

**Unstage** files / Undo `git add`

```
git restore --staged {file_name}
```

## Undo the last commit -- `git reset`

Discard commits in a **private** branch or throw away uncommitted changes

Undo the last commit and **remove the changes**
```
git reset --hard HEAD~1
```

Undo the last 2 commit and **remove the changes**
```
git reset --hard HEAD~2
```

Roll back to multiple / certain commit and **remove the changes**
```
git reset --hard <commit-id>
```

Only reset a specific file
```
git reset HEAD~2 foo.py
```

Flags: 

- `--soft` – The staged snapshot and working directory are not altered in any way.
- `--mixed` – The staged snapshot is updated to match the specified commit, but the working directory is not affected. This is the default option.
- `--hard` – The staged snapshot and the working directory are both updated to match the specified commit.

## `git revert`

**Undo** commits in a **public** branch

```
git checkout hotfix git revert HEAD~2
```

## `git rm`

Remove directory and all its content
```
git rm -rv <directory>
```

## `git fetch`

Fetch commits from remote repo
```bash
# Check update for a specific branch
git fetch origin /*location*/ main /*branch name*/

# Check update from all branches
git fetch origin
```

## Fetch a remote branch and move to the branch

New method:
```bash
git fetch origin remote-branch-name
git switch remote-branch-name
```

Old method:
```bash
# Shorthand version
git checkout --track origin/remote-branch-name

# The full length version
git check -b name-the-local-branch origin/remote-branch-name
```

## `git merge`

Merge remote commits to local main branch
```
git merge --no-ff origin/main
```