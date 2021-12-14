ARG BASE_IMAGE="rayproject/ray:1.9.0-py38-cpu"
FROM ${BASE_IMAGE}
# FROM directive resets ARG
ARG BASE_IMAGE
# If this arg is not "autoscaler" then no autoscaler requirements will be included
ARG AUTOSCALER="autoscaler"
ENV TZ=America/Los_Angeles
USER root
RUN mv /home/ray/anaconda3/ /root/anaconda3
ENV PATH "/root/anaconda3/bin:$PATH"
ENV HOME=/root

RUN apt-get update -y \
    && apt-get install -y sudo tzdata \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

RUN apt-get update -y && sudo apt-get upgrade -y \
    && sudo apt-get install -y \
        git \
        wget \
        cmake \
        g++ \
        zlib1g-dev \
        vim \
        $(if [ "$AUTOSCALER" = "autoscaler" ]; then echo \
        tmux \
        screen \
        rsync \
        openssh-client \
        gnupg; fi) \
        unzip 

RUN apt-get update -y \
    && apt-get install -y gnupg curl golang wget make git libseccomp-dev \
    && . /etc/os-release \
    && echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_${VERSION_ID}/ /" | tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list \
    && curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_${VERSION_ID}/Release.key | apt-key add - \
    && apt-get update \
    && apt-get -y install podman runc \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

RUN $HOME/anaconda3/bin/pip install --no-cache-dir ipython 

# ARG RAY_UID=1000
ARG RAY_GID=100
# USER $RAY_UID
# ENV HOME=/home/ray

# For Click
ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8
