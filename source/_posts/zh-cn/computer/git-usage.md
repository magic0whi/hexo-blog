---
title: git-usage
toc: true
category: computer
date: 2022-05-02 18:08:14
tags:
---

Some notes on Git

<!-- more -->

## Difference between ^ and ~

`commit^n` choose the nth parent of the commit

`HEAD^` = `HEAD^1` = `HEAD~` = `HEAD~1`

```plaintext
G   H   I   J
 \ /     \ /
  D   E   F
   \  |  / \
    \ | /   |
     \|/    |
      B     C
       \   /
        \ /
         A

A =      = A^0
B = A^   = A^1     = A~1
C = A^2
D = A^^  = A^1^1   = A~2
E = B^2  = A^^2
F = B^3  = A^^3
G = A^^^ = A^1^1^1 = A~3
H = D^2  = B^^2    = A^^^2  = A~2^2
I = F^   = B^3^    = A^^3^
J = F^2  = B^3^2   = A^^3^2
```

See [What's the difference between HEAD^ and HEAD~ in Git?](https://stackoverflow.com/a/2222920)

## Clone 

Clone a repository with its submodules (`--recurse-submodules`)
```console
git clone --recurse-submodules git@domain.tld:repo.git`
```

## Add & Move & Remove

Add file in a interactive patch way (`-p`/`--patch`)
```console
git add -p <file>
```

Move (Rename) file and then stage: `git mv <filename1> <filename2>`
Remove file: `git rm <file>`
Remove file in index only: `git rm --cached <file>`

## Commit

Automatically stage changes then commit: `git commit -a`

Fast modify last commit: `git commit --amend`

## Log

Show log in a friendly format: `git log --graph --oneline --decorate --all`

Show commits with diff patch (`-p`/`-u`/`--patch`/`--cc` (dense)): `git log -cc`, `git log --cc -1` (For HEAD only)

Show commits for a user (`--author=`/`--committer=`): `git log --author=<pattern>`

Grep in commit logs: `git log --grep=<pattern>`

## Diff

Show a summary: `git diff --stat`
Show diff of the last commit: `git diff HEAD HEAD~1` or `git diff HEAD^!`

## Blame

Show each line of a file with last commit hash and author: `git blame <file>`

## Stash

`git stash`
`git stash pop`

## Branch & Remove

Create a branch and set its upstream (`-t`/`--track`): `git branch <new-branch> <remote-branch>`
Delete a branch (`-d`/`--delete`): `git branch -d <branch>`
Delete a remote branch (`-d`/`--delete`): `git push <remote> -d <branch>` / `git push <remote> :<branch>`
Rename a branch (`-m`/`--move`): `git branch -m <old-branch> <new-branch>`
To rename a remtoe branch, first delete the remote branch, then push a branch with new name: `git push <remote> :<old-branch>; git push <remote> <new-branch>`

Add remote and choose a branch to track: (`-t`/`--track`): `git remote add -t <remote-branch> <remote-name> <url>`

## Tag

tag current HEAD: `git tag <tag-name>`
push tags (under `refs/tags`): `git push --tags`

## Rebase

Modify last 10 commits: `git rebase -i HEAD~10`

- `pick` doesn't change this commit
- `drop` drop this commit
- `edit` edit this commit
- `squash` merge this commit with one commit above

## Reset & Clean

Reset work tree and delete untracked files: `git reset --hard HEAD; git clean -fdx`

Reset:
- `--soft` only change HEAD to `<commit>`
- `--mixed` reset index only.
- `--hard` reset both index and working tree
- `--keep` reset working tree only (If a file differ between `<commit>` and `HEAD`, and also has modified copy in index, this reset will be abort)

Clean:
- `-f --force` allow `git clean` to delete files or directories while `clean.requireForce` is not set to false.
- `-d` Normally, when no <path> is set, `git clean` will not recurse into untracked directories to removing too much. Specify `-d` to have it recurse into such directories as well.
- `-x` Remove ignored files and directories mentioned in `.gitignore`

## Restore

Restore a file from specify commit: `git restore -s <commit> <file>`

## Rvert

Create a revert commit (Useful when in a team): `git revert <commit>`
                                                         k
## Submodule

Show submodule status: `git submodule status`
Add a submodule: `git submodule add -b <remote-branch> [--name <name>] <url>`
Initialize and update submodule to superproject's recorded commit SHA-1: `git submodule update --init` or `git submodule init; git submodule update`
Update submodule to its remote-tracking branch: `git submodule update --remote <submodule-name>`

Specify how differences in submodule are shown when using `git diff`: `git diff --submodule=[diff|log|short]`

Push only when submodule commits are available on remote-tracking branch: `git push --recurse-submodules=check`
Push also submodule commits: `git push --recurse-submodules=on-demand`

Run command in each submodules: `git submodule foreach <command>`

### More Greater Submodule for Git

使用[git-subrepo](https://github.com/ingydotnet/git-subrepo)

然后使用`git subrepo`为开头

还可将子目录变为子仓库:

```console
$ git subrepo init <subdir> [-r <remote>] [-b <branch>] [--method <merge|rebase>]
```


## Git Cherry-pick

git cherry-pick用于把另一个本地分支的commit修改应用到当前分支

假设`dev`分支有一个hash为`38361a68`的commit
```
$ git checkout master
$ git cherry-pick 38361a68
```

## Git Format-patch

1. Create patch files between two commit
   ```console
   $ git format-patch <r1>..<r2>
   ```
2. Single commit patch
   ```console
   $ git format-patch -1 <r1>
   ```
3. Create patch file since commit r1 (But doesn't include this commit)
   ```console
   $ git format-patch <r1>
   ```
4. Apply series of patches
   ```console
   $ git am *.patch
   ```


## Limit memory usage of git-gc

```console
$ git config --global pack.windowMemory "100m"
$ git config --global pack.packSizeLimit "100m"
$ git config --global pack.threads "1"
```

