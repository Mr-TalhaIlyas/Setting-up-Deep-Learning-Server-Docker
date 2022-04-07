# Opening Remote Container in VS Code

1. First Download [VS Code](https://code.visualstudio.com/download)

2. Install following extensions

* Remote Developement
* Docker
* SSH Host
* Remote Container
* Remote Explorer

3. After the installation is complete press `F1` or `Ctrl + P` to open search tab and type `Remote-SSH: Connect to Host...`
4. Click then add new host ip address where container is located.
e.g. you want to connect to the hose machine have following details,
```
IP :  192.168.1.1
username : tom
```
then click `+ add host` as follows `ssh tom@192.168.1.1` Now the VS code will show the option for viewing the `config` file. You can open it and view it to check if everything is ok. If your port is 22 i.e., default then don't change anything but if you port is other than default 22 then add the variable in `config` file as follows and save it.
```
Port 1234
```

5. Install Docker and WSL2 by follwoing instructions below,

* [Docker](https://docs.docker.com/desktop/windows/install/)

* [WSL2](https://docs.microsoft.com/en-us/windows/wsl/install-manual)

6. Now go to VS code press `F1` or `ctrl + P` and search for `Preferences: Open Settings (JSON)` open this file and add follwoing line in that file.

```
"docker.host":"ssh://root@192.168.1.1"
```
**Important** The containers must be running on the host machine for you to attach them via VS code.
* first attach to remote host via normally
* then connect to remote container 
this way you don't have to get `ssh-keygen` from powershell and set it in the host machine.

7. Now press `F1` or `ctrl + P` and search for `Remote-Containers: Attach to Running Container` and you will see a list of running containers to select from

8. Now first connect to host via `Remote-SSH : Connect to host` then connect to the running containers in host machine.

### Some useful reference links

1. [Remote Container](https://seokhyun2.tistory.com/48)
2. [Remote Host](https://seokhyun2.tistory.com/42)
3. [Pycharm/Spyder](https://blog.naver.com/lccandol/222217850151)