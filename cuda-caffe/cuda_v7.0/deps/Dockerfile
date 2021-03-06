# Start with cuDNN base image
FROM nvidia/cuda:7.0-cudnn4-devel
MAINTAINER Kai Arulkumaran <design@kaixhin.com>

# Install git, wget, bc and dependencies
RUN apt-get update && apt-get install -y \
  git \
  wget \
  bc \
  cmake \
  libatlas-base-dev \
  libatlas-dev \
  libboost-all-dev \
  libopencv-dev \
  libprotobuf-dev \
  libgoogle-glog-dev \
  libgflags-dev \
  protobuf-compiler \
  libhdf5-dev \
  libleveldb-dev \
  liblmdb-dev \
  libsnappy-dev \
  python-dev \
  python-pip \
  python-numpy \
  gfortran > /dev/null

# Install NCCL for multi-GPU communication
RUN cd /root && git clone https://github.com/NVIDIA/nccl.git && cd nccl && \
  make CUDA_HOME=/usr/local/cuda -j"$(nproc)" && \
  make PREFIX=/root/nccl install
ENV LD_LIBRARY_PATH=/root/nccl/lib:$LD_LIBRARY_PATH
ENV NCCL_INCLUDE_DIR=/root/nccl/include
ENV NCCL_LIBRARY=/root/nccl/lib

# Clone Caffe repo and move into it
RUN cd /root && git clone https://github.com/BVLC/caffe.git && cd caffe && \
# Install python dependencies
  cat python/requirements.txt | xargs -n1 pip install
