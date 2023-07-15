# SSH

\-L local forwarding

\-R remote forwarding

## **Local TCP forwarding**

![image](https://habrastorage.org/web/682/bbf/053/682bbf05346443ec9074a50a35c76c30.png)

```
host1$ ssh -L 9999:localhost:5432 host2
```



![image](https://habrastorage.org/web/6f4/2d2/af8/6f42d2af88084934b08d66c81561980b.png)

```
host1$ ssh -L 9999:host3:5432 host2
```



![image](https://habrastorage.org/web/82d/7f8/345/82d7f834541042758af4fd65ba86aca9.png)

```
host1$: ssh -L 0.0.0.0:9999:host3:5432 host2
```

* port 9999 will be opened on all IP4 available host1 addresses

## **Remote TCP forwarding**

What if host2 haven\`t got white IP address or he's behind the NAT, or all input connection to host2 are closed, or we can't install ssh server on host2 because of windows.

We can establish backward connection. From host2 toward host1. ![image](https://habrastorage.org/web/0ed/834/76e/0ed83476e544420facb0898f014c854a.png)

```
host2$ ssh -R 9999:localhost:5432 host1
```

## **TCP forwarding chain**

host1 → host2 → host3 → host4

![image](https://habrastorage.org/web/0fe/32b/cf5/0fe32bcf5e4b43e78bf7beff99ce0d8e.png)

```
host1# ssh -L 9991:localhost:9992 host2
host2# ssh -L 9992:localhost:9993 host3
host3# ssh -L 9993:localhost:5432 host4
```

* we can use one port if it's free in all hosts.

check ssh connection to git

```
$ ssh -T git@git.moscow.alfaintra.net
$ ssh -vT git@git.moscow.alfaintra.net    --- verbose
```

`chmod 600 id_rsa`
