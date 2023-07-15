# Base will install runtime dependencies and configure generics
FROM node:latest

# Install ffmpeg and other deps
RUN apt-get -qq update > /dev/null && \
    apt-get -qq -y install python3 make g++ tar xz-utils > /dev/null && \
    rm -rf /var/lib/apt/lists
RUN wget -qO /tmp/ffmpeg.tar.xz https://johnvansickle.com/ffmpeg/releases/ffmpeg-release-$(dpkg --print-architecture)-static.tar.xz && \
    tar -x -C /usr/local/bin --strip-components 1 -f /tmp/ffmpeg.tar.xz --wildcards '*/ffmpeg' && \
    rm /tmp/ffmpeg.tar.xz

# create an app user
ARG USERNAME
ARG DOCKER_UID
ARG DOCKER_GID
ARG NODE_ENV
RUN groupadd -g $DOCKER_GID $USERNAME
RUN useradd -m -u $DOCKER_UID -g $DOCKER_GID -s /bin/bash $USERNAME

WORKDIR /app

RUN chown -R $DOCKER_UID:$DOCKER_UID /app
RUN chmod -R g+w /app \
    && chown -R $USERNAME:$USERNAME /usr/local/bin/

# sudo power (without password) <- DEVELOPMENT AND PLAYGROUND ONLY
RUN usermod -a -G sudo $USERNAME && \
    echo '$USERNAME ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# starter script
COPY start-container /usr/local/bin/start-container
RUN chmod +x /usr/local/bin/*

USER ${USERNAME}
ENV NODE_ENV=${NODE_ENV}

CMD ["start-container"]