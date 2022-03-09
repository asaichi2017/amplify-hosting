FROM amazonlinux:2

# https://docs.docker.com/engine/reference/builder/#maintainer-deprecated
LABEL maintainer="Amazon AWS"

# Framework Versions
ENV VERSION_NODE_16=16
ENV VERSION_NODE_DEFAULT=$VERSION_NODE_16
ENV VERSION_BUNDLER=2.0.1
ENV VERSION_YARN=1.22.0
ENV VERSION_AMPLIFY=7.6.8

# UTF-8 Environment
ENV LANGUAGE en_US:en
ENV LANG=en_US.UTF-8
ENV LC_ALL en_US.UTF-8

## Install OS packages
RUN touch ~/.bashrc
RUN yum -y update && \
    yum -y install \
    autoconf \
    automake \
    bzip2 \
    bison \
    bzr \
    cmake \
    expect \
    fontconfig \
    git \
    gcc-c++ \
    libnotify-devel \
    libpng \
    libpng-devel \
    libffi-devel \
    libtool \
    libxml2 \
    libxml2-devel \
    libXScrnSaver \
    libyaml \
    libyaml-devel \
    make \
    nss-devel \
    openssl-devel \
    openssh-clients \
    patch \
    procps \
    readline-devel \
    sqlite-devel \
    tar \
    tree \
    unzip \
    wget \
    which \
    zip \
    zlib \
    zlib-devel \
    yum clean all && \
    rm -rf /var/cache/yum

## Install Node
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
RUN /bin/bash -c ". ~/.nvm/nvm.sh && \
    nvm install $VERSION_NODE_16 && nvm use $VERSION_NODE_16 && chown -R root:root /root/.nvm &&  \
    npm install -g yarn@${VERSION_YARN} && \
    nvm alias default ${VERSION_NODE_DEFAULT} && nvm cache clear"

# Handle yarn for any `nvm install` in the future
RUN echo "yarn@${VERSION_YARN}" > /root/.nvm/default-packages

## Install AWS Amplify CLI for all node versions
RUN /bin/bash -c ". ~/.nvm/nvm.sh && nvm use ${VERSION_NODE_16} && \
    npm config set user 0 && npm config set unsafe-perm true && \
    npm install -g @aws-amplify/cli@${VERSION_AMPLIFY}"

# Environment Setup
RUN echo export PATH="\
/root/.nvm/versions/node/${VERSION_NODE_DEFAULT}/bin:\
$PATH" >> ~/.bashrc && \
    echo "nvm use ${VERSION_NODE_DEFAULT} 1> /dev/null" >> ~/.bashrc

ENTRYPOINT [ "bash", "-c" ]
