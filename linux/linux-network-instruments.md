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





### tcp keepalive linux configuration



tcp\_keepalive\_time — интервал времени с момента отправки последнего пакета с данными; по истечении этого срока соединение помечается как требующее проверки; после начала проверки параметр не используется;&#x20;

tcp\_keepalive\_intvl — интервал между проверочными пакетами (отправка которых начинается по истечении tcp\_keepalive\_time);&#x20;

tcp\_keepalive\_probes — количество неподтвержденных проверочных пакетов; по исчерпании этого счетчика соединение считается разорванным.

```
cat /proc/sys/net/ipv4/tcp_keepalive_time
cat /proc/sys/net/ipv4/tcp_keepalive_probes
cat /proc/sys/net/ipv4/tcp_keepalive_intvl
```



Modify this config in file _`/etc/sysctl.conf`_

```
net.ipv4.tcp_keepalive_time = 300
net.ipv4.tcp_keepalive_intvl = 75
net.ipv4.tcp_keepalive_probes = 9
```

Or on the fly using command

```
echo 300 > /proc/sys/net/ipv4/tcp_keepalive_time
```



Источник проблемы лежит в природе протокола TCP/IP. Как правило, источник сеанса TCP/IP и его приемник находятся в различных сетях, и на пути сеанса встречается несколько маршрутизаторов. Хотя бы один из них обычно выполняет NAT-преобразование адресов. Ресурсы маршрутизатора всегда ограничены, поэтому некоторые из них выполняют [очистку NAT-таблиц от «мёртвых» сеансов.](#user-content-fn-1)[^1] Сеанс считается «мёртвым», если по нему не передавались пакеты в течение некоторого заданного интервала времени (назовем его интервал очистки). Таким образом, «молчаливый» сеанс может быть принят за «мёртвый» и вычищен из NAT-таблицы. При этом ни источник, ни приемник об этом не уведомляются («не царское дело»), и оба они остаются в уверенности, что сеанс ещё «жив» (в чем легко убедиться командой netstat, выполнив ее на клиенте или на сервере в момент возникновения ошибки, но до нажатия на ОК). Когда пользователь, получивший сообщение об ошибке, нажмет на ОК, о разрыве сеанса узнает клиент; серверный же процесс завершится, когда «умерший» сеанс распознает ОС.

Экспериментально установлено, что интервал очистки у многих маршутизаторов (по крайней мере, с прошитым Linux 2.4/iptables) составляет около 10 минут. Постараемся заставить наш TCP-сеанс автоматически поддерживать себя в активном состоянии, даже когда не передается никаких пакетов с данными.

[^1]: 
