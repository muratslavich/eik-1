# Memory and Space

## Disk usage



### disk-free - show available and used disk space

```
$ df -[h|a|T|i]
-h - human-readable format
-a - shows complete disk usage
-T - for each block filesystems type
-i - shows used and free nodes
```

### disk-usage - shows files and folders usage in kb

```
$ du -[h|a|s]
-h - human readable format
-a - disk usage for all files
-s - total disk usage for a particular file or directory
```

###

### stat - size and other statistics of file or dir

```
$ stat .
```

### fdisk - shows disk size along with disk partitioning information

```
$ fdisk -l 
```

## Memory usage



### top - real-time view of a running system

```
# sort all processes by memory used
$ top -o %MEM
```

### free - shows the amount of free and used memory of system

```
$ free
$ free -m  # in megabytes
$ free -g  # in gigabytes
$ free -t  # add total row
           total       used        free         shared       buff/cache     available
Mem:       131524128   109148132   4189864      519912       18186132       20551624
Swap:      67108860    18731456    48377404
```

### vmstat - memory statistics

```
$ vmstat -s
```
