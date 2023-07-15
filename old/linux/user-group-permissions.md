# user/group permissions

![CSC128: Permissions and Links: chmod and ls](https://lh3.googleusercontent.com/proxy/V7hwIfKIr9o3K3B-L86kBq-amGBfHzr0hx27acIsKYGkwRr91hIuaR6pQ3B6PKhjUInM6Phx6\_dYrMLD0xtyaipwuHgh59u2QyMwp3g\_cXqvenLNOjlm-g)

![chmod Cheatsheet: linux](https://i.redd.it/vkxuqbatopk21.png)

```
$ chmod 777 filename
$ chown user:group filename
```

## User and Group



```
// see all users
$ cat /etc/passwd

$ sudo useradd wsluser

// show user id and groups
$ id wsluser 
uid=1000(wsluser) gid=1000(wsluser) groups=1000(wsluser),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),117(netdev),1001(docker)

// delete user
$ sudo userdel testuser
// delete user with own directory
$ sudo deluser --remove-home testuser
```

```
// see all groups
$ cat /etc/group

// create new group
$ sudo groupadd section

// add user to group
$ sudo usermod -aG section testuser
$ usermod -aG sudo username

// remove user from group
$ sudo gpasswd -d testuser section

// delete group
$ sudo groupdel section
```

### sudoers

```
$ sudo cat /etc/sudoers

// edit this file
$ sudo visudo

// prefer to add user file to /etc/sudoers.d/ directory
$ sudo usermod -aG sudo username
```

uid=0(root) gid=0(root) groups=0(root)
