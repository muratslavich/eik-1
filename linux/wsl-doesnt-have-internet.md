# WSL doesn't have internet

Inside wsl distro

1. sudo vi
2. Remove line out the
3. add this new line
4. Paste the following in:

```
[network]
generateResolvConf = false
```

.wslconfig

```
[wsl2]
memory=4GB # Limits VM memory in WSL 2 to 4 GB
processors=2 # Makes the WSL 2 VM use two virtual processors

[network]
localhostForwarding=true
```

1. чтобы был доступ в интернет надо прописать правило фаервола (под админом конечно, кстати это вроде могут скоро прикрыть &#x20;

```
New-NetFirewallRule -DisplayName "WSL" -Direction Inbound -InterfaceAlias "vEthernet (WSL)" -Action Allow
```

1. отключить генерацию resolv.conf /etc/wsl.conf и на винде в {user}/.wslconfig

```
[network]
generateResolvConf = false
```

1. прописать днс руками/etc/resolv.conf

```
nameserver 8.8.8.8
nameserver 10.226.0.5
nameserver 10.224.0.5
search moscow.alfaintra.net alfaintra.net
```

1. погасить wsl&#x20;
2. отрубить сетевой интерфейс&#x20;
3. !!! всегда сначала включать ВПН, а потом сеть wsl )))



Open Command Prompt as an Administrator and type these commands:

```
netsh winsock reset 
netsh int ip reset all
netsh winhttp reset proxy
ipconfig /flushdns
```

Reboot your machine.

[https://github.com/microsoft/WSL/issues/3438#issuecomment-410518578](https://github.com/microsoft/WSL/issues/3438#issuecomment-410518578)
