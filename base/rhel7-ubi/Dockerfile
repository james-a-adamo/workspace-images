FROM registry.access.redhat.com/ubi7/ubi:7.9

COPY install-packages /usr/local/bin

### base ###
ARG RHEL_FRONTEND=noninteractive

RUN yum install -y wget && cd /tmp && cd /tmp && \
yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
yum install -y http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-2.noarch.rpm && \
yum clean all

RUN yes | unminimize \
    && install-packages \
        zip \
        unzip \
        bash-completion \
        bash-completion-extras \
        gcc \
        gcc-c++ \
        kernel-devel \
        make \
        ninja-build \
        htop \
        jq \
        less \
        locales \
        man-db \
        nano \
        cargo \
        sudo \
        time \
        emacs-nox \
        vim \
        multitail \
        lsof \
        openssl \
        fish \
        git \
        git-lfs

RUN cargo install ripgrep

ENV PATH="/root/.cargo/bin:/usr/local/bin/:$PATH"
ENV LANG=en_US.UTF-8

### Gitpod user ###
# '-l': see https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#user
RUN groupadd sudo && useradd -l -u 33333 -G sudo -md /home/gitpod -s /bin/bash -p gitpod gitpod \
    # passwordless sudo for users in the 'sudo' group
    && sed -i.bkp -e 's/%sudo\s\+ALL=(ALL\(:ALL\)\?)\s\+ALL/%sudo ALL=NOPASSWD:ALL/g' /etc/sudoers
ENV HOME=/home/gitpod
WORKDIR $HOME
# custom Bash prompt
RUN cd /uar/local/bin && wget https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh
RUN { echo && echo "PS1='\[\033[01;32m\]\u\[\033[00m\] \[\033[01;34m\]\w\[\033[00m\]\$(__git_ps1 \" (%s)\") $ '" ; } >> .bashrc

### Gitpod user (2) ###
USER gitpod
# use sudo so that user does not get sudo usage info on (the first) login
RUN sudo echo "Running 'sudo' for Gitpod: success" && \
    # create .bashrc.d folder and source it in the bashrc
    mkdir /home/gitpod/.bashrc.d && \
    (echo; echo "for i in \$(ls \$HOME/.bashrc.d/*); do source \$i; done"; echo) >> /home/gitpod/.bashrc

# configure git-lfs
RUN sudo git lfs install --system
