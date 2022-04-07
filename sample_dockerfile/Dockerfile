FROM tensorflow/tensorflow:2.3.0-gpu

RUN mkdir /app/
COPY *.* /app/
WORKDIR /app/

RUN apt-get update && apt-get install -y \
    ffmpeg \
    libsm6 \
    libxext6


RUN pip install -r requirements.txt --use-feature=2020-resolver
