FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04
ADD environment.yml /
ENV LANG C.UTF-8
ENV PATH /opt/conda/bin:$PATH
RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    PIP_INSTALL="python -m pip --no-cache-dir install --upgrade" && \
    CONDA_INSTALL="conda install -y" && \
    GIT_CLONE="git clone --depth 10" && \

    rm -rf /var/lib/apt/lists/* \
           /etc/apt/sources.list.d/cuda.list \
           /etc/apt/sources.list.d/nvidia-ml.list && \

    apt-get update && \

# ==================================================================
# tools
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        build-essential \
        apt-utils \
        ca-certificates \
        wget \
        git \
        vim \
        curl \
        wget \
        bzip2 \
        ca-certificates \
        locales \
        fonts-liberation \
        inkscape \
        jed \
        libsm6 \
        libxext-dev \
        libxrender1 \
        lmodern \
        netcat \
        pandoc \
        texlive-fonts-recommended \
        texlive-generic-recommended \
        texlive-latex-base \
        texlive-latex-extra \
        texlive-xetex \
        ffmpeg \
        graphviz\
        nano \
        vim \
        emacs \
        htop \
        zip \
        unzip \
        libssl-dev \
        libncurses5-dev \
        libncursesw5-dev \
        libopenmpi-dev \
        libopenblas-base \
        libopenblas-dev \
        libomp-dev \
        libjpeg-dev \
        libpng-dev \
        libboost-all-dev \
        libsdl2-dev \
        libcurl4-openssl-dev \
        openssh-client \
        openssh-server \
        swig \
        pkg-config \
        g++ \
        zlib1g-dev \
        protobuf-compiler \
        libosmesa6-dev \
        patchelf \
        xvfb \
        && \

    $GIT_CLONE https://github.com/Kitware/CMake ~/cmake && \
    cd ~/cmake && \
    ./bootstrap && \
    make -j"$(nproc)" install && \

# ==================================================================
# tensorflow GPU support
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive apt-key adv --fetch-keys \
        http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub \
        && \
    wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_9.1.85-1_amd64.deb \
        && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        ./cuda-repo-ubuntu1604_9.1.85-1_amd64.deb \ 
        && \
    wget http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64/nvidia-machine-learning-repo-ubuntu1604_1.0.0-1_amd64.deb \
        && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \ 
        ./nvidia-machine-learning-repo-ubuntu1604_1.0.0-1_amd64.deb \
        && \
    apt update && \

# Install CUDA and tools. Include optional NCCL 2.x
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        cuda9.0 \
        cuda-cublas-9-0 \
        cuda-cufft-9-0 \
        cuda-curand-9-0 \
        cuda-cusolver-9-0 \
        cuda-cusparse-9-0 \
        libcudnn7=7.2.1.38-1+cuda9.0 \
        libnccl2=2.2.13-1+cuda9.0 \
        cuda-command-line-tools-9-0 \
        && \

# Optional: Install the TensorRT runtime (must be after CUDA install)
    apt update && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        libnvinfer4=4.1.2-1+cuda9.0 \
        && \


# ==================================================================
# debian
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        software-properties-common \
        && \
    add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        libatlas-base-dev \
        graphviz \
        && \

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        protobuf-compiler \
        libprotoc-dev \
        && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        libblas-dev \
        && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        libatlas-base-dev \
        libgflags-dev \
        libgoogle-glog-dev \
        libhdf5-serial-dev \
        libleveldb-dev \
        liblmdb-dev \
        libprotobuf-dev \
        libsnappy-dev \
        protobuf-compiler \
        && \

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        openmpi-bin \
        libpng-dev \
        libjpeg-dev \
        libtiff-dev \
        && \

# ==================================================================
# R
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9 \
        && \
        add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu xenial-cran35/' \
        && \
        apt update \
        && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        r-base \
        r-base-dev \
        && \
# ==================================================================
# python
# ------------------------------------------------------------------
    wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda && \
    rm ~/miniconda.sh && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc

RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    PIP_INSTALL="python -m pip --no-cache-dir install --upgrade" && \
    CONDA_INSTALL="conda install -y" && \
    GIT_CLONE="git clone --depth 10" && \

    # create the environment
    conda update -n base conda -y && conda env update && \


# ==================================================================
# boost
# ------------------------------------------------------------------

#    wget -O ~/boost.tar.gz https://dl.bintray.com/boostorg/release/1.69.0/source/boost_1_69_0.tar.gz && \
#    tar -zxf ~/boost.tar.gz -C ~ && \
#    cd ~/boost_* && \
#    ./bootstrap.sh && \
#    ./b2 install -j"$(nproc)" --prefix=/usr/local && \

# ==================================================================
# pytorch
# ------------------------------------------------------------------

    $PIP_INSTALL \
        torch_nightly -f \
        https://download.pytorch.org/whl/nightly/cu90/torch_nightly.html \
        && \

# ==================================================================
# config & cleanup
# ------------------------------------------------------------------

    ldconfig && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* ~/*


EXPOSE 8888 6006
