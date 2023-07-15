# ansible.cfg ignored in wsl

os: ubuntu (windows 10) ansible: 2.6.4&#x20;

When you launch ansible-playbook and you have a local ansible.cfg, ubuntu throws this error:

```
[WARNING] Ansible is being run in a world writable directory  (/ mnt / c / code / ansible /....), ignoring it asan ansible.cfg source. For more information see https://docs.ansible.com/ansible/devel/reference_appendices/config.html#cfg-in-world-writable-dir

ERROR! Invalid or no config file was supplied
```

The problem is related to the permissions that are given to the files by default, since in fact the latest versions see these permissions (777) as a security problem and ignore the file.&#x20;

To avoid this problem just re-set the permissions of the wsl (windows subsystem linux) by creating the file

```
[Automount] 
enabled = true 
mountFsTab = false 
root = /mnt/ 
options = "metadata,umask = 22,fmask = 11"

...
```

source: [https://github.com/ansible/ansible/issues/42388#issuecomment-403926971](https://github.com/ansible/ansible/issues/42388#issuecomment-403926971)
