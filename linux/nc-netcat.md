# nc NetCat

nc command - reads and writes data across network.

```
$ nc [options] host port

-z only scan port
-v verbose

$ nc -zv corpint7 2181
Ncat: Connected to 10.211.98.10:2181.
Ncat: 0 bytes sent, 0 bytes received in 0.01 seconds.

// отправить команду
$ echo "EXIT" | nc 10.10.8.8 22
SSH-2.0-OpenSSH_7.6p1 Ubuntu-4
Protocol mismatch.
```

## Sending Files through Netcat

Netcat can be used to transfer data from one host to another by creating a basic client/server model.

This works by setting the Netcat to listen on a specific port (using the -l option) on the receiving host and then establishing a regular TCP connection from the other host and sending the file over it.

On  the receiving run the following command which will open the port 5555  for incoming connection and redirect the output to the file:

```
nc -l 5555 > file_name
```

From the sending host connect to the receiving host and send the file:

```
nc receiving.host.com 5555 < file_name
```

To transfer a directory you can use tar to archive the directory on the source host and to extract the archive on the destination host.

On the receiving host, set the Netcat tool to listen for an incoming connection on port 5555. The incoming data is piped to the [tar](https://linuxize.com/post/how-to-create-and-extract-archives-using-the-tar-command-in-linux/) command, which will extract the archive:

```
nc -l 5555 | tar xzvf -
```

On the sending host pack the directory and send the data by connecting to the listening nc process on the receiving host:

```
tar czvf - /path/to/dir | nc receiving.host.com 5555
```

You can watch the transfer progress on both ends. Once completed, type CTRL+C to close the connection.

|### Netcat Fundamentals

* `nc [options] [host] [port]`

By default this will execute a port scan

* `nc -l [host] [port]`

Initiates a listener on the given port |### Netcat File Transfer

* `nc [host] [port] > file_name.out`

Send a file

* `nc [host] [port] > file_name.in`

Receive a file | |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| |### Netcat Backdoor Shells

* `nc -l -p [port] -e /bin/bash`

Run a shell on Linux

* `nc -l -p [port] -e cmd.exe`

Run a shell on Netcat for Windows

Then from any other system on the network, you can test how to run commands on host after successful [Netcat connection in bash](https://www.varonis.com/blog/the-difference-between-bash-and-powershell/).

* **`nc -nv [host] [port]`**

|### Netcat Port Scanner

* `nc -zv site.com 80`

Scan a single port - ping

* `nc -zv hostname.com 80 84`

Scan a set of individual ports

* `nc -zv site.com 80-84`

Scan a range of ports | | | |
