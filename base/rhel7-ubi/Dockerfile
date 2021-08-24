FROM registry.access.redhat.com/ubi7/ubi:7.9

### base ###
RUN yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
yum install -y http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-2.noarch.rpm && \
yum clean all

RUN yum install -y \
        wget \
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
        git-lfs && \
        yum clean all

ENV LANG=en_US.UTF-8

### Gitpod user ###
# '-l': see https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#user
RUN useradd -l -u 33333 -G wheel -md /home/gitpod -s /bin/bash -p gitpod gitpod \
    # passwordless sudo for users in the 'wheel' group
    && sed -i.bkp -e 's/%wheel\s\+ALL=(ALL\(:ALL\)\?)\s\+ALL/%wheel ALL=NOPASSWD:ALL/g' /etc/sudoers
ENV HOME=/home/gitpod
WORKDIR $HOME
# custom Bash prompt
RUN wget -P /usr/local/bin/ https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh && echo "source /usr/local/bin/git-prompt.sh" >> .bashrc && \
    { echo && echo "PS1='\[\033[01;32m\]\u\[\033[00m\] \[\033[01;34m\]\w\[\033[00m\]\$(__git_ps1 \" (%s)\") $ '" ; } >> .bashrc
    
### Gitpod user (2) ###
USER gitpod

# Installing ripgrep and cargo-cache
RUN cargo install ripgrep cargo-cache && cargo cache -a
ENV PATH="$HOME/.cargo/bin:/usr/local/bin/:$PATH"

# use sudo so that user does not get sudo usage info on (the first) login
RUN sudo echo "Running 'sudo' for Gitpod: success" && \
    # create .bashrc.d folder and source it in the bashrc
    mkdir /home/gitpod/.bashrc.d && \
    (echo; echo "for i in \$(ls \$HOME/.bashrc.d/*); do source \$i; done"; echo) >> /home/gitpod/.bashrc

# configure git-lfs
RUN sudo git lfs install --system
