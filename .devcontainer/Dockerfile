FROM elixir:1.15.7-otp-25

LABEL org.opencontainers.image.description="Elixir devcontainer for VSCode"
LABEL org.opencontainers.image.source=https://github.com/hspaans/elixir-devcontainer

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# This Dockerfile adds a non-root user with sudo access. Use the "remoteUser"
# property in devcontainer.json to use it. On Linux, the container user's GID/UIDs
# will be updated to match your local UID/GID (when using the dockerFile property).
# See https://aka.ms/vscode-remote/containers/non-root-user for details.
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN apt-get update \
    && apt-get -y install --no-install-recommends apt-utils dialog wget sudo git lsb-release ca-certificates inotify-tools zsh \
    && groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/zsh --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    # Clean up
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /workspace
VOLUME /workspace

USER $USERNAME
ENV HOME /home/$USERNAME

RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.5/zsh-in-docker.sh)" -- \
    -t bira

# Switch back to dialog for any ad-hoc use of apt-get
ENV DEBIAN_FRONTEND=dialog

# Install Phoenix & Ecto
RUN mix local.hex --force -if-missing && mix archive.install --force hex phx_new
