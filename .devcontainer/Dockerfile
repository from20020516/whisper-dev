FROM python:3.9.9-slim-bullseye as base

RUN apt-get update && apt-get install -y \
    alsa-utils \
    build-essential \
    ffmpeg \
    gcc \
    git \
    libasound2 \
    libasound2-plugins \
    pulseaudio \
    pulseaudio-utils \
    sox \
    vorbis-tools

RUN python3 -m pip install \
    soundcard \
    numpy \
    git+https://github.com/openai/whisper.git

#

FROM node:16 as node
FROM base as development

COPY --from=node /usr/local/bin/node /usr/local/bin/node
COPY --from=node /usr/local/lib/node_modules/ /usr/local/lib/node_modules
RUN ln -s /usr/local/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm \
    && ln -s /usr/local/lib/node_modules/npm/bin/npx-cli.js /usr/local/bin/npx
COPY --from=amazon/aws-cli /usr/local/aws-cli/v2/current /usr/local
COPY --from=docker /usr/local/bin/docker /usr/local/bin/docker
COPY --from=docker /usr/local/bin/docker-compose /usr/local/bin/docker-compose

ENTRYPOINT [ "/bin/bash" ]
