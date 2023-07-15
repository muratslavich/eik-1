# Ansible

Playbook: myConfig.yaml

```
---
hosts: webserver
tasks:
    -name: install apache
    yum:
        name: apache
        state: present
    -name: staret apache
    service: 
        name: apache
        state: start
```

Master server - who?

* service configurations
* deploy managing
* setup and updates apps
* logs

Over ssh - means do not require hosts app

setup ansible to localhost [(Ubuntu)](https://docs.ansible.com/ansible/latest/installation\_guide/intro\_installation.html)

```
$ sudo apt update
$ sudo apt install software-properties-common
$ sudo apt-add-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible
```

ansible.cfg can be here:

* ./ansible.cfg – current location;
* \~/.ansible.cfg — home;
* /etc/ansible/ansible.cfg — package manager location

most used ansible params:

* hostfile: — path to inventory file, contain list of hosts ip-addresses (or names) for connection to;
* library: — path to Ansible modules;
* forks: — threads number;
* sudo\_user: — user for executing commands on remote hosts;
* remote\_port: —  SSH port;
* host\_key\_checking: — on/off SSH–key checking on remote host;
* timeout: —  SSH connection timeout;
* log\_path: — path to log file.

to define hosts name use

* /etc/ansible/hosts

```
[test]
test-1
test-1
```

ssh keys configurations

* add key on master - ssh-keygen
* copy key to host - ssh-copy-id user@server

ping hosts

```
$ ansible host_name -m ping
```

RAM usage information

```
$ ansible host_name -a "free -h"
```

## **Playbooks**

* start document with ---
* list element starts with - or space
* comments #

can contain

* list of tasks
* list of remote hosts
* user vars

Task uses some part of module, which responds by JSON.

Each instructions set ( `tasks:` ) must consist `hosts` attribute, except roles.

.instal\_nginx.yaml

```
---
- hosts: test
  tasks:
  - name: Install package nginx
    apt: name=nginx update_cache=yes
    sudo: yes
  - name: Starting service nginx
    service: name=nginx state=started
    sudo: yes
```

Find out, which hosts will work on this instructions set

```
$ ansible-playbook <имя_набора_инструкций> --list-host
```

Execute playbook

```
$ ansible-playbook <playbook_name.yaml>
```

Execute concrete task from book

```
$ ansible-playbook <playbook_name.yaml> --step --start-at-task="<task_name>"
```

* GATHERING FACTS - the first task which implicitly exist in each instructions set.

See variables and them values

```
$ ansible -m setup <hosts>
```

Include playbook to another book

```
---
- hosts: test
  tasks:
  - include: update_os.yml
```

## Variables

some\_var.yaml

```
---
package_name: "apache2"
```

include vars file to playbook

```
  vars_files:
    - /etc/ansible/vars/some_var.yml
```

can define vars directly in playbooks

```
  vars:
    - package_name: "apache2"
```

See variables and them values

```
$ ansible -m setup <hosts>
```

## Modules

[All modules list](https://docs.ansible.com/ansible/latest/modules/list\_of\_all\_modules.html)

most popular

* `command:`
* `shell:`
* `script:`
* `raw:`
* `file:`
* `copy:`
* `cron:`
* `template:`

## Roles

Loops

```
---
- hosts: test
  tasks:
 
  - name: Install packages
    apt: name={{ item }} state=latest
    with_items:
    - htop
    - mytop
    - wget
    sudo: yes
```

`with_items` - apt will execute for each element in list.

Conditions

```
---
- hosts: test
  tasks:
 
  - name: Check OS family
    debug: msg="This is my OS"
    when: ansible_os_family == "Debian"
 
  - name: Check if Apache2 is installed
    command: dpkg-query -W apache2
    register: apache2_check
 
  - name: Print message if apache installed
    debug: msg="Apache2 is installed on remote host"
    when: "'apache2' in apache2_check.stdout"
 
  - name: Check if admin logged
    command: who
    register: who_check
 
  - name: Print message if user admin not logged
    debug: msg="User admin is not logged on remote host"
    when: not 'admin' in who_check.stdout
```

`when`

* \==
* !=
* \>
* <
* \>=
* <=
* and
* or
* in
* not

Roles

In project architecture it\`s always divide to vary server layers - db, web for example

Project structure

```
site.yml
servers.yml
roles/
   web/
     files/
     templates/
     tasks/
     handlers/
     vars/
     defaults/
     meta/
   db/
     files/
     templates/
     tasks/
     handlers/
     vars/
     defaults/
     meta/
```

Folder and structure can be different.

Path to folder with roles can be define in ansible config file `roles_path`

* если существует
* если существует
* если существует
* если существует
* задача копирования может ссылаться на файл в
* скриптовая задача может ссылаться на скрипт в
* задача шаблонизации может ссылаться на
* импортируемые задачи могут ссылаться на файлы в
