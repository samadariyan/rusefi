FROM ubuntu:22.04

# Prevent interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Set build arguments and environment variables
ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8
ENV TZ=UTC

# Install required packages in a single layer to reduce image size
RUN apt-get update && apt-get install -y --no-install-recommends \
    make \
    automake \
    autoconf \
    gcc-arm-none-eabi \
    binutils-arm-none-eabi \
    git \
    nano \
    openjdk-17-jdk-headless \
    mtools \
    dosfstools \
    xxd \
    openssh-server \
    cmake \
    rsync \
    gdb-multiarch \
    build-essential \
    g++ \
    gcc \
    zip \
    zsh \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Create non-root user with a specific UID/GID
ARG USERNAME=rusefi
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && yes password | passwd $USERNAME

# Configure SSH server
RUN mkdir /var/run/sshd \
    && sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config \
    && sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config

# Set up workspace directory with proper permissions
WORKDIR /workspace
RUN chown $USERNAME:$USERNAME /workspace

# Switch to non-root user by default
USER $USERNAME

# Start SSH service and shell using proper JSON format
CMD ["sh", "-c", "sudo service ssh start && /bin/bash"]
