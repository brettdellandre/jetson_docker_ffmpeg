FROM ubuntu:20.04

ARG S6_OVERLAY_VERSION=v2.1.0.0
ARG S6_OVERLAY_ARCH=aarch64

RUN \
# Update and get dependencies
    apt-get update && \
    apt-get install -y \ 
    curl \
    git \
    unrar \
    wget \
    tzdata \
    automake \
    cmake \
    make \
    gcc \
    pkg-config \
    libegl1-mesa-dev \
    libv4l-dev \
    && \
    curl -J -L https://github.com/just-containers/s6-overlay/releases/download/${S6_OVERLAY_VERSION}/s6-overlay-${S6_OVERLAY_ARCH}.tar.gz | tar zxvf - -C / && \
    git clone https://github.com/jocover/jetson-ffmpeg.git && \
    cd jetson-ffmpeg && \
    mkdir build && \
    cd build && \
    cmake .. && \
    make && \
    sudo make install && \
    sudo ldconfig && \
    git clone git://source.ffmpeg.org/ffmpeg.git -b release/4.2 --depth=1 && \
    cd ffmpeg && \
    wget https://github.com/jocover/jetson-ffmpeg/raw/master/ffmpeg_nvmpi.patch && \
    git apply ffmpeg_nvmpi.patch && \
    ./configure --enable-nvmpi --enable-nvv4l2dec --enable-libv4l2 --enable-shared --extra-libs="-L/usr/lib/aarch64-linux-gnu/tegra -lnvbuf_utils" --extra-cflags="-I /usr/src/jetson_multimedia_api && \
    make && \
    apt-get remove -y git curl unrar tzdata wget automake cmake make \
CMD         ["--help"]
ENTRYPOINT  ["ffmpeg"]
COPY --from=build /usr/local /usr/local/
