FROM nvidia/cuda:12.2.2-cudnn8-runtime-ubuntu22.04 as diffusers-dreambooth-dev

# init

ARG TYPE="diffusers-dreambooth-dev"

LABEL maintainer="gabr1elt"
LABEL repository="diffusers"
LABEL type=$TYPE

# switch to root

USER root
ARG USERNAME="root"
ARG HOME="/"$USERNAME
WORKDIR $HOME

ENV DEBIAN_FRONTEND=noninteractive

# install default packages

RUN apt-get update && \
    # apt-get install -y --no-install-recommends \
    apt-get install -y \
        # utils
        aptitude \
        bash \
        sudo \
        xsel \
        ranger \
        byobu \
        nano \
        htop \
        nmon \
        build-essential \
        git \
        git-lfs \
        curl \
        wget \
        gnupg \
        ca-certificates \
        libsndfile1-dev \
        libgl1 \
        pkg-config \
        software-properties-common \
        unzip \
        # data
        visidata \
        graphviz \
        lnav \
        # db
        sqlite \
        sqlite3-tools \
        # vision
        tesseract-ocr \
        ## java ##
        # default-jdk-headless \
        # default-jdk \
        # openjdk-11-jdk \
        openjdk-21-jdk \
        # default-jre-headless \
        # default-jre \
        # openjdk-11-jre \
        # openjdk-21-jre \
        ## javascript ##
        nodejs \
        npm \
        ## python ##
        python3 \
        python3-pip \
        python3-wheel \
        python3-venv \
        python3-openpyxl \
        python3-arrow && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*


# Installs Node via nvm (Node Version Manager)

RUN curl -fsSL -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash && \
    # load nvm
    \. $HOME/.nvm/nvm.sh && \
    nvm install --lts && \
    node -v && \
    npm -v


# Install CUDA packages

RUN apt-get update && \
    # apt-get install -y --no-install-recommends \
    apt-get install -y \
        # TensorRT Version (import tensorflow.compiler as tf_cc; print(tf_cc.tf2tensorrt._pywrap_py_utils.get_linked_tensorrt_version())): https://github.com/tensorflow/build/blob/master/tensorflow_runtime_dockerfiles/gpu.packages.txt
        # tensorrt && \
        libnvinfer-plugin8=8.6.1.6-1+cuda12.0 \
        libnvinfer8=8.6.1.6-1+cuda12.0 && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*


# install openvscode server

ARG OPENVSCODE_SERVER_ROOT="/opt/openvscode-server"
# ARG OPENVSCODE_SERVER_RELEASE_TAG="openvscode-server-insiders-v1.90.0"

RUN RELEASE_TAG="${OPENVSCODE_SERVER_RELEASE_TAG:-$(curl -fsSL -o /dev/null -w %{url_effective} https://github.com/gitpod-io/openvscode-server/releases/latest | cut -d '/' -f 8)}" && \
    curl -fsSL https://github.com/gitpod-io/openvscode-server/releases/download/${RELEASE_TAG}/${RELEASE_TAG}-linux-x64.tar.gz -o /tmp/${RELEASE_TAG}-linux-x64.tar.gz && \
    tar -xzf /tmp/${RELEASE_TAG}-linux-x64.tar.gz -C /tmp && \
    rm -f /tmp/${RELEASE_TAG}-linux-x64.tar.gz && \
    mv -f /tmp/${RELEASE_TAG}-linux-x64 ${OPENVSCODE_SERVER_ROOT} && \
    ln -s  ${OPENVSCODE_SERVER_ROOT}/bin/remote-cli/openvscode-server ${OPENVSCODE_SERVER_ROOT}/bin/remote-cli/code && \
    ln -s  ${OPENVSCODE_SERVER_ROOT}/bin/openvscode-server /usr/local/bin/code


# install python packages

# make sure to use venv
RUN python3 -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

# pre-install the heavy dependencies (these can later be overridden by the deps from setup.py)

RUN python3 -m pip install --no-cache-dir \
        py-cpuinfo \
        ipykernel \
        ipywidgets \
        python-dotenv \
        Jinja2 \
        lxml \ 
        odfpy \ 
        openpyxl \
        pyarrow \ 
        matplotlib \
        seaborn \
        plotly \
        requests \
        streamlit \
        pillow \
        opencv-python \
        pytesseract \
        numpy \
        scipy \
        pandas \
        numexpr \ 
        bottleneck \ 
        numba \ 
        scikit-learn \
        xgboost \
        keras \
        SQLAlchemy \ 
        pymysql
        # psycopg2 \ 
        # mplfinance \
        # pandas_ta \
        # TA-Lib \
        # alpaca-py \
        # pymarketstore

RUN python3 -m pip install --no-cache-dir \
        torch \
        torchvision \
        torchaudio \
        pytorch-lightning \
        accelerate \
        datasets \
        # hf-doc-builder \
        # huggingface-hub \
        # hf_transfer \
        invisible_watermark \
        librosa \
        tensorboard \
        transformers \
        diffusers \
        safetensors \
        torch-tb-profiler \
        compel


# # fix for 'Could not load library libcudnn_cnn_infer.so.8'
# RUN apt-get update && \
#    # apt-get install -y --no-install-recommends \
#    apt-get install -y \
#        python3-opencv \
#        nvidia-cudnn && \
#    apt-get clean -y && \
#    rm -rf /var/lib/apt/lists/*


# config git

RUN git config --global user.email "torcat.m.gabriel@gmail.com" && \
    git config --global user.name "gabr1elt"

# end


# wokdir
ARG WORKSPACE_ROOT=$HOME"/Development"
RUN mkdir -p $WORKSPACE_ROOT

WORKDIR $WORKSPACE_ROOT

ENV LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    EDITOR=code \
    VISUAL=code \
    GIT_EDITOR="code --wait" \
    WORKSPACE_ROOT=${WORKSPACE_ROOT}

EXPOSE 3000
CMD [ "code", "--host", "0.0.0.0", "--without-connection-token" ]

