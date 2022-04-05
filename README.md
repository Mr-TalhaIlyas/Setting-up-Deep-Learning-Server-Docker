# Setting-up-Deep-Learning-Server-Docker

[![Linux](https://svgshare.com/i/Zhy.svg)](https://svgshare.com/i/Zhy.svg) [![made-with-bash](https://img.shields.io/badge/Made%20with-Bash-1f425f.svg)](https://www.gnu.org/software/bash/) [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2FMr-TalhaIlyas%2FSetting-up-Deep-Learning-Server-Docker&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

# Setting-Up-Deep-Learning-Server-Docker
for setting deep learning server via Anaconda go [here](https://github.com/Mr-TalhaIlyas/Deep-Learning-Sever-via-Docker)
Assuming that you already have a machine runnning Linux/Ubuntu.

## 1. Install GPU drivers

1st type the following command to get the list of recommended drivers for your PC.

```
ubuntu-drivers devices
```

![alt text](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda/blob/main/Pictures/s1.png)

Now install GPU drivers. I will install 470v drivers as shown in above images, so lets proceed.

```
// Update repository.  
$ sudo add-apt-repository ppa:graphics-drivers/ppa  
$ sudo apt update  

// Check recommeded driver can be used.  
$ apt-cache search nvidia | grep nvidia-driver-470 
```

![alt text](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda/blob/main/Pictures/s2.png)

Now lets install drivers using APT

```
// Install driver by apt.  
$ sudo apt-get install nvidia-driver-470  

// Reboot.  
$ sudo reboot  
```

※ During NVIDIA installation process if an error occurs or you can't proceed or you can't get your desired vesion to be displayes or run you have to uninstall it completelyl by

```
$ sudo apt --purge autoremove nvidia*
```
after installation verify it by

```
nvidia-smi
```
![alt text](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda/blob/main/Pictures/s3.png)

# Install Docker
**side note** When you are setting up a deep learning server you don't have to install Nvidia CUDA Toolkit. As different version of libraries require differnt versions of CUDA and cuDNN toolkits installed. Docker has advantage that when we run a specific image of a specific version of a library all of its dependencies ,like DL SDK, CUDA toolkit, are automatically installed within that isolated container. For details [here](https://docs.nvidia.com/deeplearning/frameworks/user-guide/index.html)

![alt text](/nvidia1.png)

[image source](https://docs.nvidia.com/deeplearning/frameworks/user-guide/index.html)


**`Installing Docker`**

If you have any previous or broken installation of docker then remove it first by following steps here [uninstall docker](#uninstall-docker).

The detailed steps for installing docker via different methods on `Linux` are listed [here](https://docs.docker.com/engine/install/ubuntu/). I'll be using `Install using repository` method listed there.

After you have completed the installation check the installation via


# <a name="uninstall-docker">Uninstalling Docker</a>

For complete removel 

```cli
$ dpkg -l | grep -i docker

$ sudo apt-get purge -y docker-engine docker docker.io docker-ce docker-ce-cli

$ sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce
```
Images, containers, volumes, or user-created configuration files on your host will not be removed by the above instructions. Run the following commands to delete all images, containers, and volumes:

```cli
$ sudo rm -rf /var/lib/docker /etc/docker
$ sudo rm /etc/apparmor.d/docker
$ sudo groupdel docker
$ sudo rm -rf /var/run/docker.sock
```
