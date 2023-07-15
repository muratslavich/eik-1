# Linux Commands Cheat Sheet

## Hardware Information <a href="#hardware-information" id="hardware-information"></a>

[Show **bootup messages**](https://phoenixnap.com/kb/dmesg-linux):

```
dmesg
```

See **CPU information**:

```
cat /proc/cpuinfo
```

Display **free and used memory** with:

```
free -h
```

List **hardware configuration** information:

```
lshw
```

See information about **block devices**:

```
lsblk
```

[Show **PCI devices**](https://phoenixnap.com/kb/lspci-command) in a tree-like diagram:

```
lspci -tv
```

Display **USB devices** in a tree-like diagram:

```
lsusb -tv
```

Show **hardware information** from the BIOS:

```
dmidecode
```

Display **disk data** information:

```
hdparm -i /dev/disk
```

Conduct a **read-speed test** on device/disk:

```
hdparm -tT /dev/[device]
```

Test for **unreadable blocks** on device/disk:

```
badblocks -s /dev/[device]
```

[Run a disk check](https://phoenixnap.com/kb/fsck-command-linux) on an unmounted disk or partition:

```
fsck [disk-or-partition-location]
```

## Searching <a href="#searching" id="searching"></a>

[Search for a specific pattern](https://phoenixnap.com/kb/grep-multiple-strings) in a file with [grep](https://phoenixnap.com/kb/grep-command-linux-unix-examples):

```
grep [pattern] [file_name]
```

**Recursively search for a pattern** in a directory:

```
grep -r [pattern] [directory_name]
```

[Find all files and directories](https://phoenixnap.com/kb/locate-command-in-linux) **related to a particular name**:

```
locate [name]
```

List names that **begin with a specified character** **`[a]`** in a specified location **`[/folder/location]`** by using the [**`find`** command](https://phoenixnap.com/kb/guide-linux-find-command):

```
find [/folder/location] -name [a]
```

See **files larger than a specified size** **`[+100M]`** in a folder:

```
find [/folder/location] -size [+100M]
```

## File Commands <a href="#file-commands" id="file-commands"></a>

**List files** in the directory:

```
ls
```

**List all files** ([shows hidden files](https://phoenixnap.com/kb/show-hidden-files-linux)):

```
ls -a
```

[Show directory](https://phoenixnap.com/kb/pwd-linux) you are currently working in:

```
pwd
```

[Create a new directory](https://phoenixnap.com/kb/create-directory-linux-mkdir-command):

```
mkdir [directory]
```

[Remove a file](https://phoenixnap.com/kb/how-to-remove-files-directories-linux-command-line):

```
rm [file_name] 
```

**Remove a directory** recursively:

```
rm -r [directory_name]
```

**Recursively remove a directory** without requiring confirmation:

```
rm -rf [directory_name]
```

[Copy the contents of one file](https://phoenixnap.com/kb/how-to-copy-files-directories-linux) to another file:

```
cp [file_name1] [file_name2]
```

**Recursively copy the contents of one file** to a second file:

```
cp -r [directory_name1] [directory_name2]
```

**Rename** **`[file_name1]`** to **`[file_name2]`** with the command:

```
mv [file_name1] [file_name2]
```

[Create a symbolic link](https://phoenixnap.com/kb/symbolic-link-linux) to a file:

```
ln -s /path/to/[file_name] [link_name]
```

Create a **new file** using [touch](https://phoenixnap.com/kb/touch-command-in-linux):

```
touch [file_name]
```

**Show the contents** of a file:

```
more [file_name]
```

or use the [**`cat`** command](https://phoenixnap.com/kb/linux-cat-command):

```
cat [file_name]
```

Append file contents to another file:

```
cat [file_name1] >> [file_name2]
```

Display the **first 10 lines** of a file with [head command](https://phoenixnap.com/kb/linux-head):

```
head [file_name]
```

Show the **last 10 lines** of a file:

```
tail [file_name]
```

**Encrypt** a file:

```
gpg -c [file_name]
```

**Decrypt** a file:

```
gpg [file_name.gpg]
```

Show the **number of words, lines, and bytes** in a file using [wc](https://phoenixnap.com/kb/wc-linux):

```
wc
```

List number of lines/words/characters in each file in a directory with [the xargs command](https://phoenixnap.com/kb/xargs-command):

```
ls | xargs wc
```

[Cut a section of a file](https://phoenixnap.com/kb/linux-cut) and print the result to standard output:

```
cut -d[delimiter] [filename]
```

Cut a section of piped data and print the result to standard output:

```
[data] | cut -d[delimiter]
```

[Print all lines matching a pattern](https://phoenixnap.com/kb/awk-command-in-linux) in a file:

```
awk '[pattern] {print $0}' [filename]
```

**Note:** Learn also about [gawk command](https://phoenixnap.com/kb/gawk-linux), the GNU version of awk.

[Overwrite a file](https://phoenixnap.com/kb/shred-linux) to prevent its recovery, then delete it:

```
shred -u [filename]
```

[Compare two files](https://phoenixnap.com/kb/linux-diff) and display differences:

```
diff [file1] [file2]
```

[Read and execute the file content](https://phoenixnap.com/kb/linux-source-command) in the current shell:

```
source [filename]
```

[Sort file contents](https://phoenixnap.com/kb/linux-sort) and print the result in standard output:

```
sort [options] filename
```

[Store the command output in a file](https://phoenixnap.com/kb/linux-tee) and skip the terminal output:

```
[command] | tee [filename] >/dev/null
```

#### Directory Navigation <a href="#directory-navigation" id="directory-navigation"></a>

Move **up one level** in the directory tree structure:

```
cd ..
```

[Change directory](https://phoenixnap.com/kb/linux-cd-command) **to** **`$HOME`**:

```
cd
```

**Change location** to a specified directory:

```
cd /chosen/directory
```

#### File Compression <a href="#file-compression" id="file-compression"></a>

Archive an existing file:

```
tar cf [compressed_file.tar] [file_name]
```

[Extract an archived file](https://phoenixnap.com/kb/extract-tar-gz-files-linux-command-line#htoc-using-tar-utility):

```
tar xf [compressed_file.tar]
```

Create a **gzip compressed tar file** by running:

```
tar czf [compressed_file.tar.gz]
```

**Compress** a file with the **`.gz`** extension:

```
gzip [file_name]
```

#### File Transfer <a href="#file-transfer" id="file-transfer"></a>

Copy a file to a server directory securely using the [Linux scp command](https://phoenixnap.com/kb/linux-scp-command):

```
scp [file_name.txt] [server/tmp]
```

**Synchronize** the contents of a directory **with a backup directory** using the [rsync command](https://phoenixnap.com/kb/rsync-command-linux-examples):

```
rsync -a [/your/directory] [/backup/] 
```

## Users and Groups <a href="#users-and-groups" id="users-and-groups"></a>

See details about the **active users**:

```
id
```

Show **last system logins**:

```
last
```

Display who is **currently logged into the system** with the [who command](https://phoenixnap.com/kb/linux-who-command):

```
who
```

Show which users are **logged in** and **their activity**:

```
w
```

**Add a new group** by typing:

```
groupadd [group_name]
```

Add a **new user**:

```
adduser [user_name]
```

Add a **user to a group**:

```
usermod -aG [group_name] [user_name]
```

Temporarily **elevate user privileges** to superuser or root using the [sudo command](https://phoenixnap.com/kb/linux-sudo-command):

```
sudo [command_to_be_executed_as_superuser]
```

**Delete** a user:

```
userdel [user_name] 
```

[Modify user information](https://phoenixnap.com/kb/usermod-linux) with:

```
usermod
```

[Change directory group](https://phoenixnap.com/kb/chgrp-command):

```
chgrp [group-name] [directory-name]
```

## Package Installation <a href="#package-installation" id="package-installation"></a>

[List all installed packages](https://phoenixnap.com/kb/how-to-list-installed-packages-on-centos) with **`yum`**:

```
yum list installed
```

Find a package by a **related keyword**:

```
yum search [keyword]
```

Show **package information and summary**:

```
yum info [package_name]
```

Install a package using the **YUM package manager**:

```
yum install [package_name.rpm]
```

Install a package using the **DNF package manager**:

```
dnf install [package_name.rpm]
```

Install a package[ using the **APT package manager**](https://phoenixnap.com/kb/how-to-use-apt-get-commands):

```
apt install [package_name]
```

**Install** an **`.rpm`** package from a local file:

```
rpm -i  [package_name.rpm]
```

**Remove** an **`.rpm`** package:

```
rpm -e [package_name.rpm]
```

Install software from **source code**:

```
tar zxvf [source_code.tar.gz]
cd [source_code]
./configure
make
make install
```

## Process Related <a href="#process-related" id="process-related"></a>

See a **snapshot of active processes**:

```
ps
```

Show **processes in a tree-like diagram**:

```
pstree
```

Display a **memory usage map** of processes:

```
pmap
```

See [all running processes](https://phoenixnap.com/kb/top-command-in-linux):

```
top
```

[Terminate a Linux process](https://phoenixnap.com/kb/how-to-kill-a-process-in-linux) under a **given ID**:

```
kill [process_id]
```

Terminate a process under a **specific name**:

```
pkill [proc_name]
```

Terminate all processes **labelled** **“proc”**:

```
killall [proc_name]
```

**List and resume stopped jobs** in the background:

```
bg
```

Bring the most **recently suspended job to the** **foreground**:

```
fg
```

Bring a **particular job to the** **foreground**:

```
fg [job]
```

List **files opened by running processes** with [lsof command](https://phoenixnap.com/kb/lsof-command):

```
lsof
```

[Catch a system error signal](https://phoenixnap.com/kb/bash-trap-command) in a shell script:

```
trap "[commands-to-execute-on-trapping]" [signal]
```

[Pause terminal or a Bash script](https://phoenixnap.com/kb/bash-wait-command) until a running process is completed:

```
wait
```

[Run a Linux process](https://phoenixnap.com/kb/linux-nohup) in the background:

```
nohup [command] &
```

**Note:** If you want to learn more about shell jobs, how to terminate jobs or keep them running after you log off, check out our article on [how to use disown command](https://phoenixnap.com/kb/disown-command-linux).

## System Management and Information <a href="#system-management-and-information" id="system-management-and-information"></a>

Show **system information**:

```
uname -r 
```

See [kernel release information](https://phoenixnap.com/kb/check-linux-kernel-version):

```
uname -a  
```

Display **how long the system has been running**, including load average:

```
uptime 
```

See system **hostname**:

```
hostname
```

Show the **IP address** of the system:

```
hostname -i 
```

List system **reboot history**:

```
last reboot 
```

See [current time and date](https://phoenixnap.com/kb/linux-date-command):

```
date
```

Query and **change the system clock** with:

```
timedatectl 
```

Show current **calendar** (month and day):

```
cal
```

[List logged in users](https://phoenixnap.com/kb/w-command-in-linux):

```
w
```

[See which **user you are using**](https://phoenixnap.com/kb/whoami-linux):

```
whoami
```

Show **information about a particular user**:

```
finger [username]
```

[View or limit](https://phoenixnap.com/kb/ulimit-linux-command) system resource amounts:

```
ulimit [flags] [limit]
```

[Schedule a system shutdown](https://phoenixnap.com/kb/linux-shutdown-command):

```
shutdown [hh:mm]
```

Shut Down the system immediately:

```
shutdown now
```

[Add a new kernel module](https://phoenixnap.com/kb/modprobe-command):

```
modprobe [module-name]
```

## Disk Usage <a href="#disk-usage" id="disk-usage"></a>

You can use the df and du commands to [check disk space in Linux](https://phoenixnap.com/kb/linux-check-disk-space).

See **free and used space** on mounted systems:

```
df -h
```

Show **free inodes** on mounted filesystems:

```
df -i
```

Display **disk partitions, sizes, and types** with the command:

```
fdisk -l
```

See [**disk usage** for all files and directory](https://phoenixnap.com/kb/show-linux-directory-size):

```
du -ah
```

Show **disk usage of the directory** you are currently in:

```
du -sh
```

Display **target mount point** for all filesystem:

```
findmnt
```

[**Mount a device**](https://phoenixnap.com/kb/linux-mount-command):

```
mount [device_path] [mount_point]
```

## SSH Login <a href="#ssh-login" id="ssh-login"></a>

**Connect to host** as user:

```
ssh [email protected]
```

Securely **connect to host via SSH** default port 22:

```
ssh host
```

Connect to host **using a particular port**:

```
ssh -p [port] [email protected]
```

Connect to host **via telnet default port 23**:

```
telnet host
```

## File Permission <a href="#file-permission" id="file-permission"></a>

[Chown command in Linux](https://phoenixnap.com/kb/linux-chown-command-with-examples) changes file and directory ownership.

Assign **read, write, and execute permission** to everyone:

```
chmod 777 [file_name]
```

Give **read, write, and execute permission to owner**, and r**ead and execute permission to group and others**:

```
chmod 755 [file_name]
```

Assign **full permission to owner**, and **read and write permission to group and others**:

```
chmod 766 [file_name]
```

Change the **ownership of a file**:

```
chown [user] [file_name]
```

Change the **owner and group ownership of a file**:

```
chown [user]:[group] [file_name]
```

## Network <a href="#network" id="network"></a>

[List IP addresses ](https://phoenixnap.com/kb/linux-ip-command-examples)and **network interfaces**:

```
ip addr show
```

Assign an **IP address to interface eth0**:

```
ip address add [IP_address]
```

Display **IP addresses of all network interfaces** with:

```
ifconfig
```

See **active (listening) ports** with the [netstat command](https://phoenixnap.com/kb/netstat-command):

```
netstat -pnltu
```

Show **tcp** and **udp** **ports** and their programs:

```
netstat -nutlp
```

Display more **information about a domain**:

```
whois [domain]
```

Show **DNS information** about a domain using the [dig command](https://phoenixnap.com/kb/linux-dig-command-examples):

```
dig [domain] 
```

Do a **reverse lookup** **on domain**:

```
dig -x host
```

Do **reverse lookup of an IP address**:

```
dig -x [ip_address]
```

Perform an **IP lookup for a domain**:

```
host [domain]
```

Show the **local IP address**:

```
hostname -I
```

**Download a file** from a domain using the [**`wget`** command](https://phoenixnap.com/kb/wget-command-with-examples):

```
wget [file_name]
```

Receive [information about an internet domain](https://phoenixnap.com/kb/nslookup-command):

```
nslookup [domain-name]
```

[Save a remote file to your system](https://phoenixnap.com/kb/curl-command) using the filename that corresponds to the filename on the server:

```
curl -O [file-url]
```

## Variables <a href="#variables" id="variables"></a>

[Assign an integer value](https://phoenixnap.com/kb/bash-let) to a variable:

```
let "[variable]=[value]"
```

[Export a Bash variable](https://phoenixnap.com/kb/bash-export-variable):

```
export [variable-name]
```

[Declare a Bash variable](https://phoenixnap.com/kb/bash-declare):

```
declare [variable-name]= "[value]"
```

List the names of [all the shell variables and functions](https://phoenixnap.com/kb/linux-set):

```
set
```

[Display the value](https://phoenixnap.com/kb/echo-command-linux) of a variable:

```
echo $[variable-name]
```

## Shell Command Management <a href="#shell-command-management" id="shell-command-management"></a>

[Create an alias](https://phoenixnap.com/kb/linux-alias-command) for a command:

```
alias [alias-name]='[command]'
```

[Set a custom interval](https://phoenixnap.com/kb/linux-watch-command) to run a user-defined command:

```
watch -n [interval-in-seconds] [command]
```

[Postpone the execution](https://phoenixnap.com/kb/linux-sleep) of a command:

```
sleep [time-interval] && [command]
```

Create a job to be executed at a certain time (**Ctrl+D** to exit prompt after you type in the command):

```
at [hh:mm]
```

[Display a built-in manual](https://phoenixnap.com/kb/linux-man) for a command:

```
man [command]
```

Print the history of the commands you used in the terminal:

```
history
```



## pipe and out redirect

* `0` - `stdin`, the standard input stream.
* `1` - `stdout`, the standard output stream.
* `2` - `stderr`, the standard error stream.

```
command 2> /dev/null
command 2>&1 > file same command &> file
do_something 2>&1 | tee -a some_file
```

## Linux Keyboard Shortcuts <a href="#linux-keyboard-shortcuts" id="linux-keyboard-shortcuts"></a>

**Kill process** running in the terminal:

```
Ctrl + C
```

Stop **current process**:

```
Ctrl + Z
```

The process can be **resumed** in the **foreground** with **`fg`** or in the **background** with **`bg`**.

Cut **one word before the cursor** and add it to clipboard:

```
Ctrl + W
```

Cut **part of the line before the cursor** and add it to clipboard:

```
Ctrl + U
```

Cut **part of the line after the cursor** and add it to clipboard:

```
Ctrl + K
```

**Paste** from clipboard:

```
Ctrl + Y
```

**Recall last command** that matches the provided characters:

```
Ctrl + R
```

**Run** the previously recalled command:

```
Ctrl + O
```

**Exit command history** without running a command:

```
Ctrl + G
```

**Run the last command** again:

```
!!
```

**Log out** of current session:

```
exit
```

Conclusion

The more you use Linux commands, the better you will get at remembering them. Do not stress about memorizing their syntax; use our cheat sheet.

Whenever in doubt, refer to this helpful guide for the most common Linux commands.
