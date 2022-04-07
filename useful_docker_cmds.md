# Pull Run Start Stop Commands
| cmd     | operation |Flags|Example|
| ----------- | ----------- |----|---|
|`docker pull <name>`|pull the image from hub for later use|||
|`docker run <name>`|run the container|1.`-d`=False run container in detach mode i.e. background or not,</br>2. `-t` to allocate psuedo-tty(virtual terminal),</br>3. `-p` port mapping.</br>4. `-e` entry point </br>5. `-v` mounts host volume in container image, </br>6. `--name` set container name </br>7.`-rm` remove the container if it exits</br>8.`-runtime`change the runtime if its set to nvidia container can use CUDA Toolkit.</br>9.`-e` set environment variable.|1.`-d`</br>2.`-t`</br>3. `-p <host>:<container>`</br>4.`-e <name of varable>=<value> `</br>5.`-v <host_dir>:<container_dir>`</br>6.`-name`</br> 7.`-rm`</br>8. `$ docker run -it --rm -e NVIDIA_VISIBLE_DEVICES=1 --runtime=nvidia nvidia/cuda`</br>9.`$ docker run -e NVIDIA_VISIBLE_DEVICES=1 tensorflow/tensorflow`|
|`docker attach <ID/name>`|attache running container|||
|`docker restart <ID/name>`|restart container|||
|`docker stop <ID/name>`|stop container||||
|`docker rm <ID/name>`|remove container||`-f` force remove||
|`docker rmi <ID/name>`|remove image||`-f` force remove||

# Build Push Commands
| cmd     | operation |Flags|Example|
| ----------- | ----------- |----|---|
|`docker build -t <image> .`| builds the new image from *Dockerfile* present in the current dir.|`-t` tag/name of image||
|`docker push <DockerHub_user_id>/<Image_name>:<tag>`|push docker images to docker hub.||`docker push talhailyas/tensorflow:2.3.0`|
|`docker tag <prev_image_name>:<tag> <new_image_name>:<tag>`|change docker image name||`docker tag tensorflow:2.3.0 tensorflow:2.3.0-gpu`|
|`docker commit <container_name> <image_name>:<tag>`|commit changes to already built container||`docker commit tf3 tensorflow:2.3.0`|

# Other Useful Commands

| cmd     | operation |comment|
| ----------- | ----------- |----|
| `docker iamges ls`| prints the space used by each iamge||
|`docker ps -s`|shows sizes (physical/virtual) of each container when running ||
|`docker system df`|shows how many images are on host machine how many ar running and how much memory they are using/sharing with each other|adding `-v` flage here sets the verbosity of the command output|
|`docker inspect <image>`|to see stored location or WD of specific images|none|
|`docker network ls`|list networks||
|` docker exec <container_name or container_id> <command> <params>`|runs bash command outside of running container|e.g. `docker exec ttf3 apt-get update`|



# Danger Zong Commands
| cmd     | operation |comment|
| ----------- | ----------- |----|
|`docker kill $(docker ps -q)`|stop all containers|`docker ps` will list all contaniners,</br> `-q` flag will return ids of all containers and </br>`docker kill` will stop them|
|`docker rm $(docker ps -a -q)`|remove all docker containers|`-a` flag will return all the containers even the ones not running|
|`docker rmi $(docker images -q)`|remove all docker images|`rm` means remove container </br>`rmi` means remove image|
|`docker stop $(docker ps -a -q)`|stop all containers||

## Export and import containers and images.
```
// save image.  
$ // docker save <option> <file_name>.tar <image_name>:<tag>  
$ docker save -o /home/jonghyeok/data/jonghyeok.tar numpy_nvidia/cuda:10.0-base  

// load image.
$ // docker load < <file_name>.tar  
$ docker load < /home/jonghyeok/data/jonghyeok.tar  

// save container.  
$ docker export <container_name> > <file_name>.tar  

// load container.  
$ docker import <file_name>.tar
```
