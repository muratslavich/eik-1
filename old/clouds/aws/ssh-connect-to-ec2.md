# SSH connect to ec2

[topic](https://medium.com/@mandeep\_m91/how-to-ssh-to-aws-servers-using-an-ssh-config-file-4c753b87390f)

Create a security group and add inbound rules:

* HTTP from Anywhere
* HTTPS from Anywhere
* SSH from your ip

Create security key pair:

* use pem format
* save my-key.pem, you can do it only while creating the key.
* do

to connect use

```
$ ssh -i key_name.pem instance_user@instance_public_ip
```

instance\_user depends on which concrete image you use. For example

* Amazon Linux - ec2-user
* RHEL5 - root | ec2-user
* Ubuntu - ubuntu
* Fedora - fedora | ec2-user
