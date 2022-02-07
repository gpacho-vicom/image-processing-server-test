FROM nvidia/cuda:11.3.1-devel-ubuntu20.04

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && apt-get install -y \
  python3-opencv ca-certificates python3-dev git wget sudo curl libgl1 libglib2.0-0 libavdevice-dev libavfilter-dev libopus-dev libvpx-dev pkg-config libsrtp2-dev && \
  rm -rf /var/lib/apt/lists/*

# create a non-root user
ARG USER_ID=1001
RUN useradd -m --no-log-init --system --uid ${USER_ID} hpc -g sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
USER hpc

WORKDIR /home/hpc


ENV PATH="/home/hpc/.local/bin:${PATH}"
RUN wget https://bootstrap.pypa.io/get-pip.py && \
  python3 get-pip.py --user && \
  pip install --upgrade pip && \
  rm get-pip.py

COPY requirements.txt requirements.txt
RUN pip3 install --user -r requirements.txt
RUN pip3 install --user 'git+https://github.com/facebookresearch/detectron2.git'

RUN mkdir /home/hpc/.streamlit
RUN chown -R hpc /home/hpc/.streamlit
# RUN chown -R hpc /home/hpc/webrtc/data/models/yolov5

CMD bash -C '/home/hpc/start.sh';'bash'