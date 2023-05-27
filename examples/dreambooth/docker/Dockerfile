# FROM ubuntu:20.04
# FROM ubuntu:22.04
# FROM ubuntu:latest
# FROM ubuntu:rolling
# FROM nvidia/cuda:11.7.1-cudnn8-runtime-ubuntu20.04
FROM nvidia/cuda:12.0.1-cudnn8-runtime-ubuntu22.04
# FROM nvidia/cuda:12.1.1-devel-ubuntu22.04 as base

ARG TYPE="dreambooth-training-dev"

LABEL maintainer="gabr1elt"
LABEL repository="diffusers"
LABEL type=$TYPE

USER root
ARG USERNAME="root"
ARG HOME="/"$USERNAME
WORKDIR $HOME

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    # apt-get install -y --no-install-recommends \
    apt-get install -y \
        bash \
        sudo \
        ranger \
        byobu \
        build-essential \
        git \
        git-lfs \
        curl \
        wget \
        ca-certificates \
        libsndfile1-dev \
        python3 \
        python3-pip \
        python3-venv && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*

# # Creating the user and usergroup

# ARG USERNAME="user"
# ARG USER_UID=2000
# ARG USER_GID=$USER_UID
# ARG HOME="/home/"$USERNAME

# # RUN adduser --disabled-password --gecos '' $USERNAME
# RUN groupadd --gid $USER_GID $USERNAME && \
#     useradd --uid $USER_UID --gid $USERNAME -m -s /bin/bash $USERNAME

# # sudo no password
# RUN echo $USERNAME ALL=\(root\) NOPASSWD:ALL >/etc/sudoers.d/$USERNAME && \
#     chmod 0440 /etc/sudoers.d/$USERNAME

# USER $USERNAME
# WORKDIR $HOME

# install vscode server

RUN curl -fsSL "https://code.visualstudio.com/sha/download?build=stable&os=cli-alpine-x64" -o /tmp/vscode-cli.tar.gz && \
    tar -xf /tmp/vscode-cli.tar.gz -C /usr/local/bin && \
    rm /tmp/vscode-cli.tar.gz

# # install open vscode server

# ARG RELEASE_TAG="openvscode-server-v1.78.0"
# ARG OPENVSCODE_SERVER_ROOT=$HOME"/.openvscode-server"

# # Downloading the latest VSC Server release and extracting the release archive
# RUN curl -fsSL https://github.com/gitpod-io/openvscode-server/releases/download/${RELEASE_TAG}/${RELEASE_TAG}-linux-x64.tar.gz -o /tmp/${RELEASE_TAG}-linux-x64.tar.gz && \
#     tar -xzf /tmp/${RELEASE_TAG}-linux-x64.tar.gz -C /tmp && \
#     mv -f /tmp/${RELEASE_TAG}-linux-x64 ${OPENVSCODE_SERVER_ROOT} && \
#     rm -f /tmp/${RELEASE_TAG}-linux-x64.tar.gz && \
#     cp ${OPENVSCODE_SERVER_ROOT}/bin/remote-cli/openvscode-server ${OPENVSCODE_SERVER_ROOT}/bin/remote-cli/code

# install python packages

# make sure to use venv
RUN python3 -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

# pre-install the heavy dependencies (these can later be overridden by the deps from setup.py)
# RUN python3 -m pip install --no-cache-dir --upgrade pip
RUN python3 -m pip install --no-cache-dir \
        torch \
        torchvision \
        torchaudio
RUN python3 -m pip install --no-cache-dir \
        accelerate \
        datasets \
        # hf-doc-builder \
        # huggingface-hub \
        Jinja2 \
        librosa \
        numpy \
        scipy \
        tensorboard \
        transformers \
        diffusers
RUN python3 -m pip install --no-cache-dir \
        ipykernel \
        safetensors \
        torch-tb-profiler \
        compel

# fix for 'Could not load library libcudnn_cnn_infer.so.8'
RUN apt-get update && \
    # apt-get install -y --no-install-recommends \
    apt-get install -y \
        nvidia-cudnn && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*

# wokdir
ARG WORKSPACE_ROOT=$HOME"/Development"
RUN mkdir -p $WORKSPACE_ROOT

WORKDIR $WORKSPACE_ROOT

ENV LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    EDITOR=code \
    VISUAL=code \
    GIT_EDITOR="code --wait" \
#     OPENVSCODE_SERVER_ROOT=${OPENVSCODE_SERVER_ROOT} \
#     HOME=${WORKSPACE_ROOT} \
    WORKSPACE_ROOT=${WORKSPACE_ROOT}

CMD [ "code", "tunnel", "--accept-server-license-terms", "--disable-telemetry" ]
# EXPOSE 3000
# CMD [ "/bin/sh", "-c", "${OPENVSCODE_SERVER_ROOT}/bin/openvscode-server --host 0.0.0.0 --without-connection-token \"${@}\"", "--" ]
