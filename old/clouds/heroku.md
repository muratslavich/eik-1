# Heroku



install on wsl

```
$ curl https://cli-assets.heroku.com/install.sh | sh
```

ssh config

```
Host heroku.com
  HostName heroku.com
  IdentityFile /Users/jonmountjoy/.ssh/id.heroku
  IdentitiesOnly yes
```

```
open in windows
$ explorer.exe .

login to heroku without browser
$ heroku login -i

$ heroku keys:add

$ heroku keys:remove adam@workstation.local

$ heroku keys
```
