# WSL2 connect to windows localhost:port



Can't access to windows localhost in particular port from wsl2.\
For example Vagrant + VirtualBox + WSL2 + Ansible\
Unresolved:

* [https://github.com/microsoft/WSL/issues/4619#issuecomment-821142078](https://github.com/microsoft/WSL/issues/4619#issuecomment-821142078)
* [https://github.com/hashicorp/vagrant/issues/11716](https://github.com/hashicorp/vagrant/issues/11716)

\
\
Workaround: socat tools.

* Download socat for windows archive and extract it.
* Install socat in wsl2 sudo apt install socat
* In wsl2 do $ socat tcp-listen:2200,fork exec:"/mnt/c/smdev/tools/socat/socat.exe - tcp\\:localhost\\:2200"
