# Asible cheat sheet

### ad-hoc command

```
$ ansible [pattern] -m [module] -a "[module options]"
```

### ping

```
$ ansible -m ping -i inventory [host|group]
```

### file

The [ansible.builtin.file](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file\_module.html#file-module) module allows changing ownership and permissions on files. These same options can be passed directly to the `copy` module as well:

```
$ ansible webservers -m ansible.builtin.file -a "dest=/srv/foo/a.txt mode=600"
$ ansible webservers -m ansible.builtin.file -a "dest=/srv/foo/b.txt mode=600 owner=mdehaan group=mdehaan"
```

### show all play and which hosts it will be

```
$ ansible-playbook -i inventory playbook.yml --list-hosts
```

### --check

```
$ ansible-playbook playbook.yml -i ./inventories/local-inv --check
```

In check mode, Ansible runs without making any changes on remote systems.

Options for verification, including `--check`, `--diff`, `--list-hosts`, `--list-tasks`, and `--syntax-check`

### --diff

```
$ ansible-playbook foo.yml --check --diff --limit foo.example.com
```

In diff mode, Ansible provides before-and-after comparisons.



### RAM usage information

```
$ ansible host_name -a "free -h"
```



### [ansible-playbook](https://docs.ansible.com/ansible/latest/command\_guide/cheatsheet.html#id1)[](https://docs.ansible.com/ansible/latest/command\_guide/cheatsheet.html#ansible-playbook)

```
ansible-playbook -i /path/to/my_inventory_file -u my_connection_user -k -f 3 -T 30 -t my_tag -m /path/to/my_modules -b -K my_playbook.yml
```

Loads `my_playbook.yml` from the current working directory and:

* `-i` - uses `my_inventory_file` in the path provided for [inventory](https://docs.ansible.com/ansible/latest/inventory\_guide/intro\_inventory.html#intro-inventory) to match the [pattern](https://docs.ansible.com/ansible/latest/inventory\_guide/intro\_patterns.html#intro-patterns).
* `-u` - connects [over SSH](https://docs.ansible.com/ansible/latest/inventory\_guide/connection\_details.html#connections) as `my_connection_user`.
* `-k` - asks for password which is then provided to SSH authentication.
* `-f` - allocates 3 [forks](https://docs.ansible.com/ansible/latest/playbook\_guide/playbooks\_strategies.html#playbooks-strategies).
* `-T` - sets a 30-second timeout.
* `-t` - runs only tasks marked with the [tag](https://docs.ansible.com/ansible/latest/playbook\_guide/playbooks\_tags.html#tags) `my_tag`.
* `-m` - loads [local modules](https://docs.ansible.com/ansible/latest/dev\_guide/developing\_locally.html#developing-locally) from `/path/to/my/modules`.
* `-b` - executes with elevated privileges (uses [become](https://docs.ansible.com/ansible/latest/playbook\_guide/playbooks\_privilege\_escalation.html#become)).
* `-K` - prompts the user for the become password.

See [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#ansible-playbook) for detailed documentation.

[ ](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html)

\
