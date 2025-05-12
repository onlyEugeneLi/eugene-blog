# Common git commands

Push commits to remote repository

```
git push origin(remote name) main(branch name)
```

Unstage files / Undo `git add`

```
git restore --staged {file_name}
```

Remove directory and all its content
```
git rm -rv <directory>
```

Fetch commits from remote repo
```
git fetch origin /*location*/ main /*branch name*/
```

Merge remote commits to local main branch
```
git merge --no-ff origin/main
```