# hosts

**Provisioning tools** - tools are used to install and manage clusters.

```
* Ansible, Vagrant, Chef, Puppet
```

Provider - VirtualBox, VMware, AWS, or [any other provider](https://www.vagrantup.com/docs/providers/). The machine to be up by Vagrant.

Provisioners - local or remote machine, prepare infrastructure objects for service.

**Vagrant** is a virtual machine manager. The tool provides a framework and configuration format to crate and manage machines provisioned on different provisioners.

Docker-compose vs Vagrant

| Docker-compose                               | Vagrant                                         |
| -------------------------------------------- | ----------------------------------------------- |
| Define and run multi-container applications. | Tool for building and distributing environments |
| Multi container descriptor.                  | Fast development environment setup.             |

Vagrant machines could be based on VMware, VirtualBox, Docker, and other engines.
