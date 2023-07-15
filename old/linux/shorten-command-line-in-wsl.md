# Shorten command line in wsl

### To change it for the current terminal instance only

Just enter PS1='\u:\W\\$ ' and press enter.



### To change it "permanently"

In your \~/.bashrc, find the following section:

```
if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
```

Remove the @\h, and replace the \w with an uppercase \W, so that it becomes:

```
if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u\[\033[00m\]:\[\033[01;34m\]\W\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u:\W\$ '
fi
```

Save, exit, close terminal and start another to see the result.



### Tons more options!

* See
* See
