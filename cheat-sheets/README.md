# Oh My Cheat Sheet



### OhMyZSH Commands

| Command                      | Description                                                                                                                    |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
|                              |                                                                                                                                |
| <h4>tabs</h4>                | `Create a new tab in the current directory (macOS - requires enabling access for assistive devices under System Preferences).` |
| <h4>take</h4>                | `Create a new directory and change to it, will create intermediate directories as required.`                                   |
| <h4>x / extract</h4>         | `Extract an archive (supported types: tar.{bz2,gz,xz,lzma}, bz2, rar, gz, tar, tbz2, tgz, zip, Z, 7z).`                        |
| <h4>zsh_stats</h4>           | `Get a list of the top 20 commands and how many times they have been run.`                                                     |
| <h4>uninstall_oh_my_zsh</h4> | `Uninstall Oh-my-zsh.`                                                                                                         |
| <h4>upgrade_oh_my_zsh</h4>   | `Upgrade Oh-my-zsh.`                                                                                                           |
| <h4>exec zsh</h4>            | `Apply changes made to zshrc`                                                                                                  |



### Shell Commands

| Alias          | Command                                 |
| -------------- | --------------------------------------- |
|                |                                         |
| <h4>alias</h4> | `list all aliases`                      |
| <h4>..</h4>    | `cd ..`                                 |
| <h4>...</h4>   | `cd ../..`                              |
| <h4>....</h4>  | `cd ../../..`                           |
| <h4>.....</h4> | `cd ../../../..`                        |
| <h4>/</h4>     | `cd /`                                  |
| <h4>~</h4>     | `cd ~`                                  |
| <h4>cd +n</h4> | `switch to directory number n`          |
| <h4>-</h4>     | `cd -`                                  |
| <h4>1</h4>     | `cd -`                                  |
| <h4>2</h4>     | `cd -2`                                 |
| <h4>3</h4>     | `cd -3`                                 |
| <h4>4</h4>     | `cd -4`                                 |
| <h4>5</h4>     | `cd -5`                                 |
| <h4>6</h4>     | `cd -6`                                 |
| <h4>7</h4>     | `cd -7`                                 |
| <h4>8</h4>     | `cd -8`                                 |
| <h4>9</h4>     | `cd -9`                                 |
| <h4>md</h4>    | `mkdir -p`                              |
| <h4>rd</h4>    | `rmdir`                                 |
| <h4>d</h4>     | `dirs -v (lists last used directories)` |



### Tab Completion and Suggestions

| Tab for Autocomplete and Double Tab for Suggestions |   |
| --------------------------------------------------- | - |
|                                                     |   |
| <h4>ls -(tab)</h4>                                  |   |
| <h4>cap (tab)</h4>                                  |   |
| <h4>rake (tab)</h4>                                 |   |
| <h4>ssh (tab)</h4>                                  |   |
| <h4>sudo umount (tab)</h4>                          |   |
| <h4>kill (tab)</h4>                                 |   |
| <h4>unrar (tab)</h4>                                |   |
| <h4>...</h4>                                        |   |



### Git commands Aliases

| Alias                         | Command                                                                                                                             |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
|                               |                                                                                                                                     |
| <h4>g</h4>                    | `git`                                                                                                                               |
| <h4>ga</h4>                   | `git add`                                                                                                                           |
| <h4>gau</h4>                  | `git add --update (Also: "git add -u")`                                                                                             |
| <h4>gaa</h4>                  | `git add --all`                                                                                                                     |
| <h4>gapa</h4>                 | `git add --patch`                                                                                                                   |
| <h4>gb</h4>                   | `git branch`                                                                                                                        |
| <h4>gba</h4>                  | `git branch -a`                                                                                                                     |
| <h4>gbd</h4>                  | `git branch -d`                                                                                                                     |
| <h4>gbda</h4>                 | `git branch --no-color --merged \| command grep -vE "^(\+\|\*\|\s*(master\|develop\|dev)\s*$)" \| command xargs -n 1 git branch -d` |
| <h4>gbl</h4>                  | `git blame -b -w`                                                                                                                   |
| <h4>gbnm</h4>                 | `git branch --no-merged`                                                                                                            |
| <h4>gbr</h4>                  | `git branch --remote`                                                                                                               |
| <h4>gbs</h4>                  | `git bisect`                                                                                                                        |
| <h4>gbsb</h4>                 | `git bisect bad`                                                                                                                    |
| <h4>gbsg</h4>                 | `git bisect good`                                                                                                                   |
| <h4>gbsr</h4>                 | `git bisect reset`                                                                                                                  |
| <h4>gbss</h4>                 | `git bisect start`                                                                                                                  |
| <h4>gc</h4>                   | `git commit -v`                                                                                                                     |
| <h4>gc!</h4>                  | `git commit -v --amend`                                                                                                             |
| <h4>gca</h4>                  | `git commit -v -a`                                                                                                                  |
| <h4>gca!</h4>                 | `git commit -v -a --amend`                                                                                                          |
| <h4>gcan!</h4>                | `git commit -v -a --no-edit --amend`                                                                                                |
| <h4>gcans!</h4>               | `git commit -v -a -s --no-edit --amend`                                                                                             |
| <h4>gcam</h4>                 | `git commit -a -m`                                                                                                                  |
| <h4>gcsm</h4>                 | `git commit -s -m`                                                                                                                  |
| <h4>gcb</h4>                  | `git checkout -b`                                                                                                                   |
| <h4>gcf</h4>                  | `git config --list`                                                                                                                 |
| <h4>gcl</h4>                  | `git clone --recurse-submodules`                                                                                                    |
| <h4>gclean</h4>               | `git clean -id`                                                                                                                     |
| <h4>gpristine</h4>            | `git reset --hard && git clean -dffx`                                                                                               |
| <h4>gcm</h4>                  | `git checkout master`                                                                                                               |
| <h4>gcd</h4>                  | `git checkout develop`                                                                                                              |
| <h4>gcmsg</h4>                | `git commit -m`                                                                                                                     |
| <h4>gco</h4>                  | `git checkout`                                                                                                                      |
| <h4>gcount</h4>               | `git shortlog -sn`                                                                                                                  |
| <h4>gcp</h4>                  | `git cherry-pick`                                                                                                                   |
| <h4>gcpa</h4>                 | `git cherry-pick --abort`                                                                                                           |
| <h4>gcpc</h4>                 | `git cherry-pick --continue`                                                                                                        |
| <h4>gcs</h4>                  | `git commit -S`                                                                                                                     |
| <h4>gd</h4>                   | `git diff`                                                                                                                          |
| <h4>gdca</h4>                 | `git diff --cached`                                                                                                                 |
| <h4>gdct</h4>                 | `` git describe --tags `git rev-list --tags --max-count=1` ``                                                                       |
| <h4>gds</h4>                  | `git diff --staged`                                                                                                                 |
| <h4>gdt</h4>                  | `git diff-tree --no-commit-id --name-only -r`                                                                                       |
| <h4>gdw</h4>                  | `git diff --word-diff`                                                                                                              |
| <h4>gf</h4>                   | `git fetch`                                                                                                                         |
| <h4>gfa</h4>                  | `git fetch --all --prune`                                                                                                           |
| <h4>gfo</h4>                  | `git fetch origin`                                                                                                                  |
| <h4>gg</h4>                   | `git gui citool`                                                                                                                    |
| <h4>gga</h4>                  | `git gui citool --amend`                                                                                                            |
| <h4>ggpnp</h4>                | `git pull origin $(current_branch) && git push origin $(current_branch)`                                                            |
| <h4>ggpull</h4>               | `git pull origin $(current_branch)`                                                                                                 |
| <h4>ggl</h4>                  | `git pull origin $(current_branch)`                                                                                                 |
| <h4>ggpur</h4>                | `git pull --rebase origin $(current_branch)`                                                                                        |
| <h4>ggu</h4>                  | `git pull --rebase origin $(current_branch)`                                                                                        |
| <h4>glum</h4>                 | `git pull upstream master`                                                                                                          |
| <h4>ggpush</h4>               | `git push origin $(current_branch)`                                                                                                 |
| <h4>ggp</h4>                  | `git push origin $(current_branch)`                                                                                                 |
| <h4>ggfl</h4>                 | `git push --force-with-lease origin <your_argument>/$(current_branch)`                                                              |
| <h4>ggsup</h4>                | `git branch --set-upstream-to=origin/$(current_branch)`                                                                             |
| <h4>gpsup</h4>                | `git push --set-upstream origin $(current_branch)`                                                                                  |
| <h4>ghh</h4>                  | `git help`                                                                                                                          |
| <h4>gignore</h4>              | `git update-index --assume-unchanged`                                                                                               |
| <h4>gignored</h4>             | `git ls-files -v`                                                                                                                   |
| <h4>git-svn-dcommit-push</h4> | `git svn dcommit && git push github master:svntrunk`                                                                                |
| <h4>gk</h4>                   | `\gitk --all --branches`                                                                                                            |
| <h4>gke</h4>                  | `\gitk --all $(git log -g --pretty=%h)`                                                                                             |
| <h4>gl</h4>                   | `git pull`                                                                                                                          |
| <h4>glg</h4>                  | `git log --stat`                                                                                                                    |
| <h4>glgg</h4>                 | `git log --graph`                                                                                                                   |
| <h4>glgga</h4>                | `git log --graph --decorate --all`                                                                                                  |
| <h4>glgm</h4>                 | `git log --graph --max-count=10`                                                                                                    |
| <h4>glgp</h4>                 | `git log --stat -p`                                                                                                                 |
| <h4>glo</h4>                  | `git log --oneline --decorate`                                                                                                      |
| <h4>glog</h4>                 | `git log --oneline --decorate --graph`                                                                                              |
| <h4>glol</h4>                 | `git log --graph --pretty=\'%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset\'`                          |
| <h4>glola</h4>                | `git log --graph --pretty=\'%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset\' --all`                    |
| <h4>glp</h4>                  | `_git_log_prettily (Also: "git log --pretty=$1")`                                                                                   |
| <h4>gm</h4>                   | `git merge`                                                                                                                         |
| <h4>gma</h4>                  | `git merge --abort`                                                                                                                 |
| <h4>gmom</h4>                 | `git merge origin/master`                                                                                                           |
| <h4>gmt</h4>                  | `git mergetool --no-prompt`                                                                                                         |
| <h4>gmtvim</h4>               | `git mergetool --no-prompt --tool=vimdiff`                                                                                          |
| <h4>gmum</h4>                 | `git merge upstream/master`                                                                                                         |
| <h4>gp</h4>                   | `git push`                                                                                                                          |
| <h4>gpd</h4>                  | `git push --dry-run`                                                                                                                |
| <h4>gpoat</h4>                | `git push origin --all && git push origin --tags`                                                                                   |
| <h4>gpu</h4>                  | `git push upstream`                                                                                                                 |
| <h4>gpv</h4>                  | `git push -v`                                                                                                                       |
| <h4>gr</h4>                   | `git remote`                                                                                                                        |
| <h4>gra</h4>                  | `git remote add`                                                                                                                    |
| <h4>grb</h4>                  | `git rebase`                                                                                                                        |
| <h4>grba</h4>                 | `git rebase --abort`                                                                                                                |
| <h4>grbc</h4>                 | `git rebase --continue`                                                                                                             |
| <h4>grbd</h4>                 | `git rebase develop`                                                                                                                |
| <h4>grbi</h4>                 | `git rebase -i`                                                                                                                     |
| <h4>grbm</h4>                 | `git rebase master`                                                                                                                 |
| <h4>grbs</h4>                 | `git rebase --skip`                                                                                                                 |
| <h4>grh</h4>                  | `git reset (Also: "git reset HEAD")`                                                                                                |
| <h4>grhh</h4>                 | `git reset --hard (Also: "git reset HEAD --hard")`                                                                                  |
| <h4>grmv</h4>                 | `git remote rename`                                                                                                                 |
| <h4>grrm</h4>                 | `git remote remove`                                                                                                                 |
| <h4>grs</h4>                  | `git restore`                                                                                                                       |
| <h4>grset</h4>                | `git remote set-url`                                                                                                                |
| <h4>grt</h4>                  | `cd $(git rev-parse --show-toplevel \|\| echo ".")`                                                                                 |
| <h4>gru</h4>                  | `git reset --`                                                                                                                      |
| <h4>grup</h4>                 | `git remote update`                                                                                                                 |
| <h4>grv</h4>                  | `git remote -v`                                                                                                                     |
| <h4>gsb</h4>                  | `git status -sb`                                                                                                                    |
| <h4>gsd</h4>                  | `git svn dcommit`                                                                                                                   |
| <h4>gsi</h4>                  | `git submodule init`                                                                                                                |
| <h4>gsps</h4>                 | `git show --pretty=short --show-signature`                                                                                          |
| <h4>gsr</h4>                  | `git svn rebase`                                                                                                                    |
| <h4>gss</h4>                  | `git status -s`                                                                                                                     |
| <h4>gst</h4>                  | `git status`                                                                                                                        |
| <h4>gsta</h4>                 | `git stash save`                                                                                                                    |
| <h4>gstaa</h4>                | `git stash apply`                                                                                                                   |
| <h4>gstd</h4>                 | `git stash drop`                                                                                                                    |
| <h4>gstl</h4>                 | `git stash list`                                                                                                                    |
| <h4>gstp</h4>                 | `git stash pop`                                                                                                                     |
| <h4>gstc</h4>                 | `git stash clear`                                                                                                                   |
| <h4>gsts</h4>                 | `git stash show --text`                                                                                                             |
| <h4>gsu</h4>                  | `git submodule update`                                                                                                              |
| <h4>gts</h4>                  | `git tag -s`                                                                                                                        |
| <h4>gunignore</h4>            | `git update-index --no-assume-unchanged`                                                                                            |
| <h4>gunwip</h4>               | `git log -n 1 \| grep -q -c "--wip--" && git reset HEAD~1`                                                                          |
| <h4>gup</h4>                  | `git pull --rebase`                                                                                                                 |
| <h4>gupv</h4>                 | `git pull --rebase -v`                                                                                                              |
| <h4>gvt</h4>                  | `git verify-tag`                                                                                                                    |
| <h4>gwch</h4>                 | `git whatchanged -p --abbrev-commit --pretty=medium`                                                                                |
| <h4>gwip</h4>                 | `git add -A; git rm $(git ls-files --deleted) 2> /dev/null; git commit --no-verify --no-gpg-sign -m "--wip-- [skip ci]"`            |



### Editor Shortcuts

| Alias        | Command                                                                  |
| ------------ | ------------------------------------------------------------------------ |
|              |                                                                          |
| <h4>stt</h4> | `(When using sublime plugin) Open current directory in Sublime Text 2/3` |
| <h4>v</h4>   | `(When using vi-mode plugin) Edit current command line in Vim`           |



### Symfony2 aliases

| Alias                | Command                       |
| -------------------- | ----------------------------- |
|                      |                               |
| <h4>sf</h4>          | `php ./app/console`           |
| <h4>sfcl</h4>        | `php app/console cache:clear` |
| <h4>sfcontainer</h4> | `sf debug:container`          |
| <h4>sfcw</h4>        | `sf cache:warmup`             |
| <h4>sfgb</h4>        | `sf generate:bundle`          |
| <h4>sfroute</h4>     | `sf debug:router`             |
| <h4>sfsr</h4>        | `sf server:run -vvv`          |



### tmux Aliases

| Alias         | Command                |
| ------------- | ---------------------- |
|               |                        |
| <h4>ta</h4>   | `tmux attach -t`       |
| <h4>tad</h4>  | `tmux attach -d -t`    |
| <h4>ts</h4>   | `tmux new-session -s`  |
| <h4>tl</h4>   | `tmux list-sessions`   |
| <h4>tksv</h4> | `tmux kill-server`     |
| <h4>tkss</h4> | `tmux kill-session -t` |



### systemctl aliases

| Command                  | Description                                |
| ------------------------ | ------------------------------------------ |
|                          |                                            |
| <h4>sc-status NAME</h4>  | `show the status of the NAME process`      |
| <h4>sc-show NAME</h4>    | `show the NAME systemd .service file`      |
| <h4>sc-start NAME</h4>   | `start the NAME process`                   |
| <h4>sc-stop NAME</h4>    | `stop the NAME process`                    |
| <h4>sc-restart NAME</h4> | `restart the NAME process`                 |
| <h4>sc-enable NAME</h4>  | `enable the NAME process to start at boot` |
| <h4>sc-disable NAME</h4> | `disable the NAME process at boot`         |



### Ruby On Rails Aliases

| Alias        | Command                    |
| ------------ | -------------------------- |
|              |                            |
| <h4>rc</h4>  | `rails console`            |
| <h4>rcs</h4> | `rails console --sandbox`  |
| <h4>rd</h4>  | `rails destroy`            |
| <h4>rdb</h4> | `rails dbconsole`          |
| <h4>rg</h4>  | `rails generate`           |
| <h4>rgm</h4> | `rails generate migration` |
| <h4>rp</h4>  | `rails plugin`             |
| <h4>ru</h4>  | `rails runner`             |
| <h4>rs</h4>  | `rails server`             |
| <h4>rsd</h4> | `rails server --debugger`  |
| <h4>rsp</h4> | `rails server --port`      |



### RAILS\_ENV Aliases

| Alias        | Command                 |
| ------------ | ----------------------- |
|              |                         |
| <h4>RED</h4> | `RAILS_ENV=development` |
| <h4>REP</h4> | `RAILS_ENV=production`  |
| <h4>RET</h4> | `RAILS_ENV=test`        |
