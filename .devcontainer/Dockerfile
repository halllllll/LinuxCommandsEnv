# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.238.0/containers/ubuntu/.devcontainer/base.Dockerfile

# [Choice] Ubuntu version (use ubuntu-22.04 or ubuntu-18.04 on local arm64/Apple Silicon): ubuntu-22.04, ubuntu-20.04, ubuntu-18.04
ARG VARIANT="jammy"
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>

USER root
# install
# - scdl
# - for whisper-cpp

RUN apt-get update && apt-get upgrade -y && apt-get install -y ffmpeg build-essential cmake clang libssl-dev vim

# install yt-dlp
RUN wget -qO /usr/local/bin/yt-dlp https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp
RUN chmod a+rx /usr/local/bin/yt-dlp
# update
RUN yt-dlp -U

# whisper-cpp program 
RUN git clone https://github.com/ggerganov/whisper.cpp.git && cd whisper.cpp && bash ./models/download-ggml-model.sh medium && make


# なぜか作成されない（エラーにもならないっぽい）
RUN mkdir -p drive/scdl && mkdir -p drive/yt-dlp
