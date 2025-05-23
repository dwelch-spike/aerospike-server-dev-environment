FROM ubuntu:24.04

LABEL maintainer="Aerospike dwelch@aerospike.com"
LABEL description="Aerospike Server Dev Environment for Ubuntu 24.04"

ENV DEBIAN_FRONTEND=noninteractive

# Install base + build dependencies (matches install-dependencies.sh)
RUN apt-get update && apt-get install -y --no-install-recommends \
    sudo curl gnupg ca-certificates git openssh-server vim \
    make automake autoconf libtool gcc g++ \
    cmake libssl-dev libldap2-dev pkg-config \
    zlib1g-dev libcurl4-openssl-dev meld ntp default-jre gcc-13-plugin-dev \
    && rm -rf /var/lib/apt/lists/*

# Create a non-root dev user
# ARG USERNAME=devuser
# ARG PASSWORD=devpassword
# RUN useradd -m -s /bin/bash ${USERNAME} && \
# echo "${USERNAME}:${PASSWORD}" | chpasswd && \
# adduser ${USERNAME} sudo

# Set file descriptor limits
RUN echo '* soft nofile 65536\n* hard nofile 65536\nroot soft nofile 65536\nroot hard nofile 65536' >> /etc/security/limits.conf && \
    echo 'DefaultLimitNOFILE=65536' >> /etc/systemd/system.conf && \
    echo 'DefaultLimitNOFILE=65536' >> /etc/systemd/user.conf

# Prepare SSH (optional, useful for debugging)
RUN mkdir /var/run/sshd
EXPOSE 22

# Switch to dev user and working directory
# USER ${USERNAME}
WORKDIR /workspace

# Entry shell
CMD ["/bin/bash"]
