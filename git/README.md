
## git log

```shell
# Show recent commit histories that impose changes on `filepath`
# 列出最近的commit history中,影响`filepath`的commits
git log -- filepath

# Show recent commit histories that impose changes on `filepath`, and all the changes
# 列出最近的commit history中,影响`filepath`的commits, and all the changes
git log -p --filepath

# Show file changes between `commit1` and `commit2`
# 两次commit之间某文件的改动
git diff commit1:filepath commit2:filepath
```


EXAMPLES (*from `git log --help`*)

* `git log --no-merges`

  Show the whole commit history, but skip any merges

* `git log v2.6.12.. include/scsi drivers/scsi`

  Show all commits since version v2.6.12 that changed any file in the
  include/scsi or drivers/scsi subdirectories

* `git log --since="2 weeks ago" -- gitk`

  Show the changes during the last two weeks to the file gitk.
  The "--" is necessary to avoid confusion with the branch named gitk

* `git log --name-status release..test`

  Show the commits that are in the "test" branch but not yet in the
  "release" branch, along with the list of paths each commit modifies.

* `git log --follow builtin-rev-list.c`

  Shows the commits that changed builtin-rev-list.c, including those commits
  that occurred before the file was given its present name.

* `git log --branches --not --remotes=origin`

  Shows all commits that are in any of local branches but not in any of
  remote tracking branches for origin (what you have that origin doesn't).

* `git log master --not --remotes=*/master`

  Shows all commits that are in local master but not in any remote
  repository master branches.

* `git log -p -m --first-parent`

  Shows the history including change diffs, but only from the "main branch"
  perspective, skipping commits that come from merged branches,
  and showing full diffs of changes introduced by the merges.
  This makes sense only when following a strict policy of merging all topic
  branches when staying on a single integration branch.


## github上fork别人项目后如何与源项目保持同步

Fork `sjtu-medialab/mass` to `arthurchiao/mass`. When `sjtu-medialab/mass` is changed,
i want to pull and rebase the change to `arthurchiao/mass`.

```shell
git clone https://arthurchiao/github.com/arthurchiao/mass.git

cd mass

# add upstream, the name (upstream_medialab) is arbitrary
git remote add upstream_medialab https://github.com/sjtu-medialab/mass.git

# assume on master branch
git pull upstream_medialab master

# if no conflicts, the changes will be rebased onto your code
# push changes to your own repo (https://github.com/arthurchiao/mass.git)
git push origin master
```

## Handle `pull request` and merge code in command line
If you do not want to use the merge button or an automatic merge cannot be
performed, you can perform a manual merge on the command line.

Step 1: From your project repository, check out a new branch and test the changes.
```shell
git checkout -b ArthurChiao-master master
git pull https://github.com/ArthurChiao/mass.git master
```

Step 2: Merge the changes and update on GitHub.
```shell
git checkout master
git merge --no-ff ArthurChiao-master
git push origin master
```

## git clone
```shell
# --depth <depth> Create a shallow clone with a history truncated to the specified number of revisions.
git clone --depth 1 https://github.com/imatix/zguide.git
```

## git clean
git-clean - Remove untracked files from the working tree.

Cleans the working tree by recursively removing files that are not under
version control, starting from the current directory.
```shell
git clean [-d] [-f] [-i] [-n] [-q] [-e <pattern>] [-x | -X] [--] <path>…
```

## remove local/remote branches
```
# remove local branch
git branch --delete --force <branchName>
git branch -D <branchName>

# remove remote branch
git push origin --delete <branchName>
```
