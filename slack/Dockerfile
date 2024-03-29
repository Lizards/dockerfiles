# Slack desktop app
# With xauth for X11 Magic Cookie support, Pulse Audio, socat (for xdg-open-server tool), and run as `slack` user instead of root
#
# docker build -t lizards/slack .
#
# docker run --rm --detach --name slack \
#     -v /etc/localtime:/etc/localtime:ro \
#     --ipc="host" \
#     --shm-size="1024" \
#     `# The below fixes: libudev: udev_has_devtmpfs: name_to_handle_at on /dev: Operation not permitted` \
#     --security-opt seccomp=unconfined \
#     `# xdg-open-server` \
#     -v "${XDG_RUNTIME_DIR}/xdg-open-server:${XDG_RUNTIME_DIR}/xdg-open-server:ro" \
#     -v /usr/share/xdg-open-server/xdg-open.sh:/usr/bin/xdg-open:ro \
#     `# Display ` \
#     --device /dev/dri \
#     -e DISPLAY \
#     -v /tmp/.X11-unix:/tmp/.X11-unix:ro \
#     -e "XCOOKIE=$(xauth list | grep unix | cut -f2 -d'/' | tr -cd '\11\12\15\40-\176' | sed -e 's/  / /g')" \
#     `# Sound` \
#     -e "PULSE_SERVER=unix:${XDG_RUNTIME_DIR}/pulse/native" \
#     -v "${XDG_RUNTIME_DIR}/pulse/native:${XDG_RUNTIME_DIR}/pulse/native:ro" \
#     `# dbus` \
#     -v /run/dbus:/run/dbus:ro \
#     -v "${XDG_RUNTIME_DIR}/bus:${XDG_RUNTIME_DIR}/bus:ro" \
#     -e DBUS_SESSION_BUS_ADDRESS \
#     `# GTK (custom colors in app menu)` \
#     -v /usr/share/themes/:/usr/share/themes/ \
#     -v "${HOME}/.gtkrc-2.0:/home/slack/.gtkrc-2.0" \
#     `# User mounts (config dir, mount home dir as read only for uploading stuff to Slack)` \
#     -v "${HOME}/.slack:/home/slack/.config/Slack" \
#     -v "${HOME}:/media:ro" \
#     `# Optional, hidpi support` \
#     -e DOCKER_SLACK_FORCE_DEVICE_SCALE_FACTOR \
#     --entrypoint "/bin/bash" \
#     lizards/slack \
#     -c 'touch ~/.Xauthority && xauth add $XCOOKIE && /usr/lib/slack/slack --force-device-scale-factor=$DOCKER_SLACK_FORCE_DEVICE_SCALE_FACTOR'

FROM debian:bullseye-slim

ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV HOME /home/slack

RUN apt-get update && apt-get install -y \
    apt-transport-https \
    ca-certificates \
    gnupg \
    locales \
    xauth \
    socat \
    alsa-utils \
    pulseaudio \
    libxshmfence-dev \
    libgbm-dev \
    libasound2 \
    libgtk-3-0 \
    libx11-xcb1 \
    libxkbfile1 \
    libappindicator3-1 \
    libglib2.0-bin \
    libnotify4 \
    libnss3 \
    libxss1 \
    xdg-utils \
    wget \
    --no-install-recommends \
    && rm -rf /var/lib/apt/lists/* \
    && useradd --create-home --home-dir $HOME slack \
    && chown -R slack:slack $HOME \
    && usermod -a -G audio,video slack \
    && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
    && locale-gen en_US.utf8 \
    && /usr/sbin/update-locale LANG=en_US.UTF-8

RUN echo "deb http://deb.debian.org/debian/ sid main contrib non-free" > /etc/apt/sources.list \
    && apt update \
    && apt install -y  \
    && rm /etc/apt/sources.list \
    && wget https://downloads.slack-edge.com/releases/linux/4.36.140/prod/x64/slack-desktop-4.36.140-amd64.deb \
    && apt install -y ./slack-desktop-*-amd64.deb

WORKDIR $HOME
USER slack

ENTRYPOINT ["/usr/lib/slack/slack"]
CMD []
