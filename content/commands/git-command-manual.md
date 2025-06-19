# Common git commands

## `git push`

Push commits to remote repository

```
git push origin(remote name) main(branch name)
```

## `git restore`

Unstage files / Undo `git add`

```
git restore --staged {file_name}
```

## `git reset`

Discard commits in a **private** branch or throw away uncommitted changes

Revert local merge from remote
```
git reset --hard HEAD~1
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
```
git fetch origin /*location*/ main /*branch name*/
```

## `git merge`

Merge remote commits to local main branch
```
git merge --no-ff origin/main
```