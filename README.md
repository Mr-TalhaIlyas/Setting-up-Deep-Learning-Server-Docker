# Setting-up-Deep-Learning-Server-Docker

[![Linux](https://svgshare.com/i/Zhy.svg)](https://svgshare.com/i/Zhy.svg) [![made-with-bash](https://img.shields.io/badge/Made%20with-Bash-1f425f.svg)](https://www.gnu.org/software/bash/) [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2FMr-TalhaIlyas%2FSetting-up-Deep-Learning-Server-Docker&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

# Contents

* Setting Up Deep Learning Server Docer
* [Build Custom ML Docker Images](https://github.com/Mr-TalhaIlyas/Setting-up-Deep-Learning-Server-Docker/blob/main/build_docker_image.md).
* [Useful Docker Commands](https://github.com/Mr-TalhaIlyas/Setting-up-Deep-Learning-Server-Docker/blob/main/useful_docker_cmds.md).
* Uninstall Docker

# Setting-Up-Deep-Learning-Server-Docker
for setting deep learning server via Anaconda go [here](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda)
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

## 2. Install Docker

When you are setting up a deep learning server you don't have to install Nvidia CUDA Toolkit. As different version of libraries require differnt versions of CUDA and cuDNN toolkits installed. Docker has advantage that when we run a specific image of a specific version of a library all of its dependencies ,like DL SDK, CUDA toolkit, are automatically installed within that isolated container. For details [here](https://docs.nvidia.com/deeplearning/frameworks/user-guide/index.html)

![alt text](https://github.com/Mr-TalhaIlyas/Setting-up-Deep-Learning-Server-Docker/blob/main/screens/nvidia1.png)

[image source](https://docs.nvidia.com/deeplearning/frameworks/user-guide/index.html)


**`Installing Docker`**

If you have any previous or broken installation of docker then remove it first by following steps here [uninstall docker](#uninstall-docker).

The detailed steps for installing docker via different methods on `Linux` are listed [here](https://docs.docker.com/engine/install/ubuntu/). I'll be using [`Install using repository`](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository) method listed there.

I'll be installing the latest version of Docker available, currently whihc is `5:20.10.14~3-0~ubuntu-bionic`.
After you have completed the installation check the installation and version via
```
$ sudo docker version
```
![alt text](https://github.com/Mr-TalhaIlyas/Setting-up-Deep-Learning-Server-Docker/blob/main/screens/docker_v.png)

```
$ sudo docker run docker/whalesay cowsay hello to the world of docker
```
and you'll semething like

![alt text](https://github.com/Mr-TalhaIlyas/Setting-up-Deep-Learning-Server-Docker/blob/main/screens/hello_docker.png)

Notice the yellow highlighted area that the container `whalesay` wasn't available on your local machine. So the docker automatically dowloaded it from the official docker hub.

## Post-Installation Steps on Linux for Conveninece
Notice that in the above commands we had to use `sudo` just for checking version installed and printing `docker hello world`. Docker will always need root permission. So to give it root permisison by default follow the steps below.

```
//creat new docker group
$  sudo groupadd docker
// you might see,
//groupadd: group 'docker' already exists
// but continue anyway

// here i'll replace $USER with my username i.e, talha
$  sudo usermod -aG docker $USER

// to activate groupchanges
$ newgrp docker 

// now test it, without sudo
$ docker run docker/whalesay cowsay hello to the world of docker
```
## 3. Install Nvidia Container Toolkit
Lets first install `NVIDIA Container Toolkit` whihc will allows us to build and run GPU accelerated Docker containers liek TF/Torch. Details for installation can be found [here](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker). But I'll just summarize them below.

Setup the package repository and the GPG key:
```
$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
      && curl -s -L https://nvidia.github.io/libnvidia-container/gpgkey | sudo apt-key add - \
      && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```
Now install `nvidia-docker2` package

```
$ sudo apt-get update

$ sudo apt-get install -y nvidia-docker2

// Restart the Docker daemon to complete the installation after setting the default runtime:
$ sudo systemctl restart docker
```
and you are done installing nvidia container. We haven't installed any specific `CUDA` or `cuDNN` toolkit here. Those will be automatically installed when we will install a specific version of our desired library.

## 4. Installing Deep Learning Libraries (Tensorflow/Pytorch)

We'll install here `tensorflow-gpu` first.
1. [Official TF Guide](https://www.tensorflow.org/install/docker)
2. [Docker TF Hub](https://hub.docker.com/r/tensorflow/tensorflow/)


Go [here](https://hub.docker.com/r/tensorflow/tensorflow/tags) and select your desired `tf` version. Just like `conda` needs you to specify the vesion of lib to be installed like `conda install lib_name==0.1.3` etc. Docker uses tags to specify the versions to be installed.

We will install `tensorflow-gpu==2.3.0` here. So type in the following command. The parameter `--gpys all` is important without it the gpus will not be detected by the container.

```
$ docker run --gpus all -it --rm tensorflow/tensorflow:2.3.0-gpu
```

Once you run above command you'll see

![alt text](https://github.com/Mr-TalhaIlyas/Setting-up-Deep-Learning-Server-Docker/blob/main/screens/tf3.png)

Moreover you'll see that after running the above command now you're inside a completely isolated container. As this 

![alt text](https://github.com/Mr-TalhaIlyas/Setting-up-Deep-Learning-Server-Docker/blob/main/screens/b.png)

change to this

![alt text](https://github.com/Mr-TalhaIlyas/Setting-up-Deep-Learning-Server-Docker/blob/main/screens/r.png)

Now while inside container you can check your isntallation via

```
// type this while insed container
$ python

// you'll enter python now
>>>import tensorflow as tf
>>>tf.config.list_physical_devices('GPU')
```
You'll see following output.

![alt text](https://github.com/Mr-TalhaIlyas/Setting-up-Deep-Learning-Server-Docker/blob/main/screens/tf3_op.png)

## 5. Installing `pip` libraries.

Definately you don't only need the `tf` to develope you ML models. YOu need other libraries like `opencv`, `imutils` etc. So while inside this container you can install these libraries via `pip` as you'd ususally do.

```
$ pip install fmutils
$ pip install <name of lib>
```
*Step 4.* [Exiting Container](#issues)

To exit container press `Ctrl` + `P` + `Q`

If you have entered python inside container then first press `Ctrl` + `Z`.

⚠ **Every time you exit container all of your installed libs/packages will also be destroyed as the containers don't hold any data or any OS. They only exist as long as the process inside them is running after that they are destroyed and every time you run the command for `docker run` a completely new docker container in created.** ⚠

So its better to create Docker custom image files using `Docker build` so that you can load your own custom containers with all the dependencides installed.

I have already build one container for that [here](https://hub.docker.com/r/talhailyas/tf)

```
docker pull talhailyas/tf
```

For that follow the steps mentioned here **[Build Custom ML Docker Images](https://github.com/Mr-TalhaIlyas/Setting-up-Deep-Learning-Server-Docker/blob/main/build_docker_image.md)**.

For other useful docker commands go here **[Useful Docker Commands](https://github.com/Mr-TalhaIlyas/Setting-up-Deep-Learning-Server-Docker/blob/main/useful_docker_cmds.md)**.







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
