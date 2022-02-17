ARG VARIANT="base"
FROM archlinux:${VARIANT}

ARG USERNAME=remote
ARG PASSWORD=remote
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN pacman -Sy --noconfirm \
    # required for perl, archlinux:base has an older glibc version
    glibc \
    # required for installing AUR packages
    base-devel \
    git \
    # sexy terminal
    zsh \
    # other
    sudo \
    openssh \
    # pygments
    python \
    python-pip \
    python-pygments \
    # texlive
    texlive-core \
    texlive-bibtexextra \
    texlive-formatsextra \
    texlive-latexextra \
    texlive-pictures \
    texlive-science

# create new user and allow sudo
RUN groupadd --gid ${USER_GID} ${USERNAME} && \
    useradd --uid ${USER_UID} --gid ${USER_GID} -s /usr/bin/zsh -m ${USERNAME} && \
    echo "${USERNAME}:${PASSWORD}" | chpasswd && \
    echo ${USERNAME} ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/${USERNAME} && \
    chmod 0440 /etc/sudoers.d/${USERNAME}

# generate hostkeys for sshd
RUN ssh-keygen -A

# switch to remote user to install AUR packages
USER ${USER_UID}:${USER_GID}

# install tllocalmgr (https://aur.archlinux.org/packages/tllocalmgr-git)
# install perl-term-shellui (https://aur.archlinux.org/packages/perl-term-shellui)
RUN cd && mkdir aur && cd aur && \
    git clone https://aur.archlinux.org/perl-term-shellui.git && \
    git clone https://aur.archlinux.org/tllocalmgr-git.git && \
    cd perl-term-shellui && makepkg -sircm --noconfirm && \
    cd ../tllocalmgr-git && makepkg -sircm --noconfirm

# switch back to root to run ssh daemon
USER root

# export SSH port
EXPOSE 22

# mounting point for the code
VOLUME /code

# start ssh daemon
ENTRYPOINT [ "/usr/sbin/sshd", "-D", "-e" ]