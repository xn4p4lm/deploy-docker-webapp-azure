FROM rockylinux:9

ARG USERNAME=codespace
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ARG HOMEDIR=/home/$USERNAME

# set user to root to install packages
USER root

# create groupid
RUN groupadd --gid ${USER_GID} ${USERNAME}

# Create a non-root user to use if preferred 
RUN useradd -u ${USER_UID} -g ${USER_GID} -d ${HOMEDIR} ${USERNAME}

# Update DNF
RUN dnf -y update

# Install DNF config manager
RUN dnf -y install 'dnf-command(config-manager)'

# add official docker repo
RUN dnf -y config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# add terraform repo
RUN dnf -y config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo

# groupinstall development tools 
RUN dnf -y groupinstall development

RUN dnf -y install sudo terraform docker docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Enable docker service
RUN systemctl enable docker

# Create a user for docker
RUN usermod -aG docker ${USERNAME}

# Set user for codespace to sudoers
RUN echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
