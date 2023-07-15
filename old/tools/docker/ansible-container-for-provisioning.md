# Ansible container for provisioning

https://iceburn.medium.com/run-ansible-with-docker-9eb27d75285b

```
FROM python:3.7.6-stretch

RUN pip install pip --upgrade
RUN pip3 install 'ansible==2.7.4'

RUN useradd -m user
RUN mkdir -p /home/user/.ssh
RUN chown -R user:user /home/user/.ssh

USER user

WORKDIR /work
```

* add a user to prevent access issue
* grant .ssh to user
* workdir with wanted playbook

build container

```
$ docker build -t ansible-container .
```

start row

```
docker run -v "${PWD}":/work:ro -v ~/.ansible:/home/user/.ansible -v ~/.ssh:/home/user/.ssh:ro --rm ansible-container:latest ansible-playbook -i inventory playbook.yaml -t tag
```

* volume roles, shared roles and general

create alias for command

```
~/.bashrc

# add to the end of file
# source ~/.bashrc
alias docker-ansible='docker run -v "${PWD}":/work:ro -v ~/.ansible:/home/user/.ansible -v ~/.ssh:/home/user/.ssh:ro --rm ansible-container:latest'
```

using row

```
$ ansible-docker ansible-galaxy install -r requirements.yml
$ ansible-docker ansible --version
```
