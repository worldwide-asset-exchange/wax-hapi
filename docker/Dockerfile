ARG deps_dir=.

################################################################################
FROM ubuntu:18.04 as builder  
ARG deps_dir
ARG symbol=WAX
ARG root_key=EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
        autoconf \
        automake \ 
        autotools-dev \ 
        clang \
        clang-8 clang-tools-8 libclang-common-8-dev libclang-8-dev libclang1-8 \
        cmake \
        curl \
        doxygen \
        git \
        graphviz \
        libbz2-dev \
        libc++-8-dev libc++abi-8-dev \
        libcurl4-gnutls-dev \
        libgmp3-dev \
        libicu-dev \ 
        libllvm8 llvm-8 llvm-8-dev llvm-8-runtime \
        libssl-dev \
        libtool \
        libusb-1.0-0-dev \
        lld-8 \
        llvm-4.0 \
        make \ 
        patch \
        pkg-config \
        python2.7 \
        python2.7-dev \ 
        python3 \
        python3-dev \
        ruby \
        sudo \
        zlib1g-dev && \
    rm -rf /var/lib/apt/lists/*

COPY ${deps_dir}/hapi-limited /tmp/hapi-limited
WORKDIR /tmp/hapi-limited/scripts
RUN ./eosio_build.sh -i /usr/local/hapi-limited -s ${symbol} -r ${root_key} && \
    ./eosio_install.sh
    
################################################################################    
FROM ubuntu:18.04
ARG deps_dir

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
        libssl-dev \
        libusb-1.0-0-dev \
        libcurl4-gnutls-dev && \
    rm -rf /var/lib/apt/lists/*

COPY --from=builder /usr/local/hapi-limited/bin   /usr/local/hapi-limited/bin
COPY --from=builder /usr/local/hapi-limited/etc   /usr/local/hapi-limited/etc
COPY --from=builder /usr/local/hapi-limited/share /usr/local/hapi-limited/share
COPY --from=builder /usr/local/hapi-limited/var   /usr/local/hapi-limited/var

ENV EOSIO_ROOT=/usr/local/hapi-limited
ENV PATH="/usr/local/hapi-limited/bin:${PATH}"
