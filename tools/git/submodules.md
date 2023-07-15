# submodules

We have project A and B

\-dir

\-A

In A project

`git submodule add http://pathToProjectB`

\-dir

\-A

\-pathToProjectB

\-projectBFiles

\-.gitmodules

\-projectFiles

```
# .gitmodules

[submodule "corp-jenkinsfile"]
        path = pathToProjectB
        url = http://pathToProjectB
```

Submodule directory isn't tracked by git.

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
  (commit or discard the untracked or modified content in submodules)
        modified:   .gitmodules.swp
        modified:   B (untracked content)

no changes added to commit (use "git add" and/or "git commit -a")
```

After cloning project with submodules  you get empty submodules folders.

`git submodule init` - for init local submodule file .gitmodules

`git submodule update` -    to fetch all submodule files

You can fetch submodule files If you pass `--recursive` tag to clone command

Working with submodules

1. You just get copy of subproject sometimes, but don't change it.

Get submodule changes

```
$ cd pathToProjectB
$ git fetch
$ git merge

or if you don't want do it manually

$ git submodule update --remote pathToProjectB
$ git submodule update --remote // for all submodules
```

git itself move to submodule folder and fetch + merge submodule changes, but from master branch.

if you want that git tracks  another branch, you should to specify that in .gitmodules (other users also will track this branch) or local .git/config

```
$ git config -f .gitmodules submodule.DbConnector.branch stable //in .gitmodules
$ git config submodule.DbConnector.branch stable //local
```

branch after submodule update will be in detached HEAD state.
