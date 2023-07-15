# Linux network instruments

network port

* port number
* associated IP address
* protocol type TCP/UDP

Could be closed/filtered by firewall - to not accept incoming packets

You can’t have two services listening to the same port on the same IP address.

### nmap \[Options] \[Target...]

all parameters - [https://nmap.org/book/man-briefoptions.html](https://nmap.org/book/man-briefoptions.html)

```
// scan all 65535 ports  
$ nmap -p- localhost

// scan 1000 most popular ports
$ nmap localhost

// scan concrete port
$ nmap -p 9092 localhost
PORT     STATE SERVICE
9092/tcp open  XmlIpcRegSvc

// one or more addresses
$ nmap -p 22 corpint4 corpint5

// specify range CIDR
$ nmap 10.211.98.0/24

// range by -
$ nmap -p 9092 10.211.98.1-25

$ nmap -p ssh corpint5

// -sn quick ping host without port scan
$ sudo nmap -sn corpint5
Nmap scan report for corpint5 (10.211.98.8)         <-===========- can see IP address
Host is up (0.0072s latency).

$ sudo nmap -sn 10.211.98.0/24
```

### netstat

manual - [https://man7.org/linux/man-pages/man8/netstat.8.html](https://man7.org/linux/man-pages/man8/netstat.8.html)

```
// check port avaibility
$ netstat -tulpn | grep 9092
-t - show tcp ports
-u - show udp ports
-l - show only listening
-p - show PID (sudo)
-n - address instead of host name

// answer
Proto  Recv-Q     Send-Q Local  Address                 Foreign Address         State       PID/Program name
tcp    LISTEN     0      128    [::]:9092               [::]:*

// Network interfaces list
$ netstat -i
```

### **ss**

new netstat

```
$ ss -tulpn | grep 9092
```

### telnet

```
telnet [domain name or ip] [port]
```

CIDR notation ![Understanding IP Addressing and CIDR Charts — RIPE Network Coordination  Centre](https://www.ripe.net/images/IPv4CIDRChart\_2015.jpg)Fi
