# Validating script

### ping

```
$ ansible -m ping -i inventory [host|group]
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

### --diff

```
$ ansible-playbook foo.yml --check --diff --limit foo.example.com
```

In diff mode, Ansible provides before-and-after comparisons.
