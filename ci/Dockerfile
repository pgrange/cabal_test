FROM debian:latest

RUN apt-get update && \
    # Install netbase to work around https://github.com/input-output-hk/cardano-node/issues/2752
    apt-get install -y netbase && \
    # Install ghcup dependencies
    apt-get install -y --no-install-recommends \
        curl \
        libnuma-dev \
        zlib1g-dev \
        libgmp-dev \
        libgmp10 \
        git \
        wget \
        lsb-release \
        software-properties-common \
        gnupg2 \
        apt-transport-https \
        gcc \
        autoconf \
        automake \
        build-essential && \
    rm -rf /var/lib/apt/lists/*

ADD cardano-node.tar.gz /srv/cardano

ADD ghcup /srv/bin/ghcup
RUN chmod +x /srv/bin/ghcup
ENV PATH="${PATH}:/srv/bin"


RUN \
    ghcup -v install ghc --isolate /usr/local --force 8.10.7 && \
    ghcup -v install cabal --isolate /usr/local/bin --force 3.10.1.0

# Install libsodium
WORKDIR /srv/
RUN git clone https://github.com/input-output-hk/libsodium
WORKDIR /srv/libsodium
RUN git checkout 66f017f1
RUN ./autogen.sh
RUN ./configure
RUN make
RUN make install
ENV LD_LIBRARY_PATH="/usr/local/lib:${LD_LIBRARY_PATH}"
ENV PKG_CONFIG_PATH="/usr/local/lib/pkgconfig:${PKG_CONFIG_PATH}"

# Install secp256k1
WORKDIR /srv/
RUN git clone https://github.com/bitcoin-core/secp256k1
WORKDIR /srv/secp256k1
RUN git checkout ac83be33
RUN ./autogen.sh
RUN ./configure --enable-module-schnorrsig --enable-experimental
RUN make
RUN make install

WORKDIR /src
