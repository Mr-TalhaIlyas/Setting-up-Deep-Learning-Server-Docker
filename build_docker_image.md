# Building your own (Machine/Deep Learning) Docker Image

Due to issues mentioned [here](https://github.com/Mr-TalhaIlyas/Setting-up-Deep-Learning-Server-Docker/blob/main/README.md#issues) it's better to create you own custome ML images with all the dependencies included on top of our base image. So that you don't have to install basic packages like  `opencv`, `matplotlib` etc over and over again.

For full details own official [Docker Hub](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

Here I'll summarize making docker image for ML/DL models. I'll be creating `tensorflow 2.3.0` image with gpu support on my machine.

### 1. Build a Dockerfile as follows
```Dockerfile
# base image this one will have the OS, python and pip already installed in it.
FROM tensorflow/tensorflow:2.3.0-gpu

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
docker build -t ttf3 .
```
`-t` will specify tag/name of your package build
`.` will specify that the Dockerfile is in the same dir. Else you can put full path there.

After running it will take some time to download and cache all the packages. Once first build is done the next build will be fast as Docker will use cached data.
