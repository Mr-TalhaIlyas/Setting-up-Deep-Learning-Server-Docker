# Building your own (Machine/Deep Learning) Docker Image

Due to issues mentioned [here](https://github.com/Mr-TalhaIlyas/Setting-up-Deep-Learning-Server-Docker/blob/main/README.md#issues) it's better to create you own custome ML images with all the dependencies included on top of our base image. So that you don't have to install basic packages like  `opencv`, `matplotlib` etc over and over again.

For full details own official [Docker Hub](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

Here I'll summarize making docker image for ML/DL models. I'll be creating `tensorflow 2.3.0` image with gpu support on my machine.

### 1. Build a Dockerfile as follows
```Dockerfile
# base image this one will have the OS, python and pip already installed in it.
FROM tensorflow/tensorflow:2.3.0-gpu

# first make a dir end it with '/' 
RUN mkdir /app/

# then copy the all the files in your source dir to the new dir
COPY *.* /app/

# now change the working dir of container
WORKDIR /app/

# for cache busting use both apt-get update and install in same line with &&
RUN apt-get update && apt-get install -y \  
    ffmpeg \
    libsm6 \
    libxext6


RUN pip install -r requirements.txt

COPY . /home/talha/data_ssd/Talha
```
### 2. Create `requirement.txt` file.
and the `requirement.txt` file contains following list of packages just like when installing packages via `pip`.

```txt
opencv-python
matplotlib
pillow
tqdm
fmutils
empatches
gray2color
```
include all the packages you need in this `txt` file.


### Build your image
Copy the both files in same `dir` and `cd` to that `dir` and start `build` by running following command.

```cmd
docker build -t my_doc_image .
```
`-t` will specify tag/name of your package build

`.` will specify that the Dockerfile is in the same dir. Else you can put full path there.

After running it will take some time to download and cache all the packages. Once first build is done the next build will be fast as Docker will use cached data.

### Run your image

Now you can run your image by simply typing

```
docker run -it --rm my_doc_image
```
`-it` flag wil enables interactive processes in the workbench terminal.

`--rm` flag will remove intermediate containers after a successful build.

For detailed options/flags [here](https://docs.docker.com/engine/reference/commandline/build/)

If you want to mount an external host machine `dir` to be mounted in the container then you can type

```
docker run -it --rm -v /dir/in/hose/machine/:/app my_doc_image
```
`-v` flag will mount *local folder* `/dir/in/hose/machine/` to `/app/` directory on the container image.

**Note**: we created and set the working dir to `/app/` while creating image.
