# Uncomplicated Firewall (UFW) is not blocking anything when using Docker

This is my first time setting up an Ubuntu Server (14.04 LTS) and I am having trouble configuring the firewall (UFW).

I only need `ssh` and `http`, so I am doing this:

```
sudo ufw disable

sudo ufw reset
sudo ufw default deny incoming
sudo ufw default allow outgoing

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp

sudo ufw enable
sudo reboot
```

**But I can still connect to databases on other ports of this machine**. Any idea about what am I doing wrong?

**EDIT**: these databases are on Docker containers. Could this be related? is it overriding my ufw config?

**EDIT2**: output of `sudo ufw status verbose`

```
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), deny (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
80/tcp                     ALLOW IN    Anywhere
22/tcp (v6)                ALLOW IN    Anywhere (v6)
80/tcp (v6)                ALLOW IN    Anywhere (v6)
```

asked Jul 25, 2015 at 10:00



ESalaESala

2,4612 gold badges12 silver badges12 bronze badges

2

The problem was using the `-p` flag on containers.

It turns out that Docker makes changes directly on your `iptables`, which are not shown with `ufw status`.

Possible solutions are:

1. Stop using the `-p` flag. Use docker linking or [docker networks](https://docs.docker.com/engine/userguide/networking/) instead.
2.  Bind containers locally so they are not exposed outside your machine:

    `docker run -p 127.0.0.1:8080:8080 ...`
3.  If you insist on using the `-p` flag, tell docker not to touch your `iptables` by disabling them in `/etc/docker/daemon.json` and restarting:

    `{ "iptables" : false }`

I recommend option 1 or 2. Beware that option 3 [has side-effects](https://docs.docker.com/engine/userguide/networking/default\_network/container-communication/), like containers becoming unable to connect to the internet.

answered Jul 25, 2015 at 10:50



ESalaESala

2,4612 gold badges12 silver badges12 bronze badges

10

16.04 presents new challenges. I did all the steps as shown [Running Docker behind the ufw firewall](https://svenv.nl/unixandlinux/dockerufw) **BUT** I could NOT get docker plus UFW to work on 16.04. In other words no matter what I did all docker ports became globally exposed to the internet. Until I found this: [How to set Docker 1.12+ to NOT interfere with IPTABLES/FirewallD](https://web.archive.org/web/20170403124320/http://blog.samcater.com/how-to-set-docker-1-12-to-not-interfere-with-iptables-firewalld/)

I had to create the file `/etc/docker/daemon.json` and put the following in:

```
{
    "iptables": false
}
```

I then issued `sudo service docker stop` then `sudo service docker start` FINALLY docker is simply following the appropriate rules in UFW.

Additional data: [Docker overrules UFW!](http://chjdev.com/2016/06/08/docker-ufw/)



Eliah Kagan

116k54 gold badges312 silver badges488 bronze badges

answered Dec 6, 2016 at 21:50



Hal JordanHal Jordan

991 silver badge3 bronze badges

1

If you're using the init system of systemd (Ubuntu 15.10 and later) edit the `/etc/docker/daemon.json` (might need to create it if it does not exist), make sure it has `iptables` key configured:

```
{   "iptables" : false }
```



**EDIT**: this might cause you to lose connection to the internet from inside containers

If you have UFW enabled, verify that you can access the internet from inside containers. if not - you must define `DEFAULT_FORWARD_POLICY` as `ACCEPT` on `/etc/default/ufw` and apply the trick described here: [https://stackoverflow.com/a/17498195/507564](https://stackoverflow.com/a/17498195/507564)



answered Oct 5, 2016 at 12:11



orshacharorshachar

1691 silver badge4 bronze badges

In my case I've ended up modifying iptables to allow access to Docker only from specific IPs.

As per [ESala's answer](https://askubuntu.com/posts/652572/revisions):

> If you use `-p` flag on containers Docker makes changes directly to iptables, ignoring the ufw.

**Example of records added to iptables by Docker**

Routing to 'DOCKER' chain:

```
-A PREROUTING -m addrtype --dst-type LOCAL -j DOCKER
-A OUTPUT ! -d 127.0.0.0/8 -m addrtype --dst-type LOCAL -j DOCKER
```

Forwarding packets from 'DOCKER' chain to container:

```
-A DOCKER -i docker0 -j RETURN
-A DOCKER ! -i docker0 -p tcp -m tcp --dport 6379 -j DNAT --to-destination 172.17.0.3:6379
```

**You can modify iptables to allow access to DOCKER chain only from specified source IP (e.g. `1.1.1.1`):**

```
-A PREROUTING -s 1.1.1.1 -m addrtype --dst-type LOCAL -j DOCKER
-A OUTPUT -s 1.1.1.1 ! -d 127.0.0.0/8 -m addrtype --dst-type LOCAL -j DOCKER
```

You may want to use `iptables-save > /tmp/iptables.conf` and `iptables-restore < /tmp/iptables.conf` to dump, edit, and restore iptables rules.

answered Feb 27, 2019 at 22:34



A fast workaround is when running Docker and doing the port mapping. You can always do

```
docker run ...-p 127.0.0.1:<ext pot>:<internal port> ...
```

to prevent your Docker from being accessed from outside.

answered Aug 16, 2017 at 14:01



kimy82kimy82

1313 bronze badges

### UPDATE 10.2021

When we use only one port-number then docker will use an ephemeral port see [docker-compose ports](https://docs.docker.com/compose/compose-file/compose-file-v3/#ports)

> Specify just the container port (an ephemeral host port is chosen for the host port).

I thought that my original answer worked, because I could not connect to the expected port 1234.\
So please don't do this - follow the advice in the other answers.\
I will not delete this answer, because it might still be useful to someone to understand why **not** to do this.

### original answer

I used `docker-compose` to start several containers and also had the problem that one port was exposed to the world ignoring ufw rules.

The fix to only make the port available to my docker containers was this change in my `docker-compose.yml` file:

```
ports:
- "1234:1234"
```

to this:

```
ports:
- "1234"
```

Now the other docker-containers still can use the port, but I cannot access it from outside.

answered Feb 17, 2019 at 10:00



TmTronTmTron

4163 silver badges15 bronze badges

1

Use of `/etc/docker/daemon.json` with content

```
{
  "iptables": false
}
```

might sound like a solution **but it only works until the next reboot**. After that you may notice that none of your containers has access to Internet so you can't for example ping any website. It may be undesired behavior.

Same applies to binding a container to specific IP. You may not want to do that. **The ultimate option is to create a container and have it behind UFW not matter what happens** and how you create this container, so there's a solution:

After you create `/etc/docker/daemon.json` file, invoke:

```
sed -i -e 's/DEFAULT_FORWARD_POLICY="DROP"/DEFAULT_FORWARD_POLICY="ACCEPT"/g' /etc/default/ufw
ufw reload
```

so you set up default forward policy in UFW for accept, and use:

```
iptables -t nat -A POSTROUTING ! -o docker0 -s 172.17.0.0/16 -j MASQUERADE
```

If you're about to use docker-compose, then the IP from command above should be replaced with IP of network docker-compose creates when running it with `docker-compose up`.

I described the problem and solution more comprehensively [in this article](https://www.mkubaczyk.com/2017/09/05/force-docker-not-bypass-ufw-rules-ubuntu-16-04/)

Hope it helps!



Zanna♦

68.7k55 gold badges212 silver badges322 bronze badges

answered Sep 8, 2017 at 19:47



Use --network=host when you start container so docker will map port to isolated host-only network instead of default bridge network. I see no legal ways to block bridged network. Alternatively you can use custom user-defined network with isolation.

answered Mar 11, 2017 at 21:10



1

1.  Login to your docker console:

    > sudo docker exec -i -t **docker\_image\_name** /bin/bash
2.  And then inside your docker console:

    > ```
    > sudo apt-get update
    > sudo apt-get install ufw
    > sudo ufw allow 22
    > ```
3.  Add your ufw rules and enable the ufw

    > sudo ufw enable

    * Your Docker image need to be started with --cap-add=NET\_ADMIN

To enable "NET\_ADMIN" Docker option:

1.Stop Container:

docker stop yourcontainer; 2.Get container id:

docker inspect yourcontainer; 3.Modify hostconfig.json(default docker path:/var/lib/docker, you can change yours)

```
vim /var/lib/docker/containers/containerid/hostconfig.json
```

4.Search "CapAdd", and modify null to \["NET\_ADMIN"];

....,"VolumesFrom":null,"CapAdd":\["NET\_ADMIN"],"CapDrop":null,.... 5.Restart docker in host machine;

service docker restart; 6.Start yourconatiner;

docker start yourcontainer;

answered Apr 3, 2018 at 13:03



**An addition to the accepted answer**

If you map a port like `127.0.0.1:8080:8080` to keep it closed from the Internet, but still want to be able to access it from your working environment (e.g for monitoring/administration/debugging purposes, with IP whitelist or ssh tunnel) there is a "well-known" solution for that:

[https://github.com/chaifeng/ufw-docker](https://github.com/chaifeng/ufw-docker)

answered Dec 27, 2022 at 22:54



###
