ARG DEBIAN_RELEASE
FROM mwohlert/rust-base:${DEBIAN_RELEASE}

ARG CROSS_TRIPLE=arm-linux-gnueabihf

RUN dpkg --add-architecture armhf && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
    gcc-${CROSS_TRIPLE} \
    g++-${CROSS_TRIPLE} \
    libdbus-1-dev:armhf \
    make \
    libssl-dev:armhf && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN ln -s /usr/bin/${CROSS_TRIPLE}-strip /usr/bin/cross-strip

# Setup Rust for ARMv7 cross-compilation

ARG RUST_VERSION

RUN curl https://sh.rustup.rs -sSf | sh -s -- --default-toolchain ${RUST_VERSION} -y

ENV PATH=/root/.cargo/bin:$PATH

RUN rustup target add armv7-unknown-linux-gnueabihf

ENV PKG_CONFIG_ALLOW_CROSS=1 \
    PKG_CONFIG_LIBDIR=/usr/lib/${CROSS_TRIPLE}/pkgconfig/ \
    CARGO_TARGET_ARMV7_UNKNOWN_LINUX_GNUEABIHF_LINKER=arm-linux-gnueabihf-gcc


VOLUME /work

WORKDIR /work
