# Enable chmod in WSL

[https://devblogs.microsoft.com/commandline/chmod-chown-wsl-improvements/](https://devblogs.microsoft.com/commandline/chmod-chown-wsl-improvements/)

Create this file in your wsl: /etc/wsl.conf

Content:

```
[automount]
enabled = true
mountFsTab = false
root = /mnt/
options = "metadata,umask=22,fmask=11"
```

After that all /mnt/c/foo will have different folder permissions (not 777 any more) and you will be able to use chmod. It requires you to have the latest WSL as far as I know. [wsl.conf docs](https://docs.microsoft.com/en-us/windows/wsl/wsl-config)
