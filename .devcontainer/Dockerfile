FROM devkitpro/devkitarm:latest

# Full 3DS SDK: libctru, 3dstools, portlibs, etc.
RUN dkp-pacman -Sy --noconfirm 3ds-dev

# Tools needed to install Rust and compile 3gxtool
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        build-essential cmake git curl ca-certificates pkg-config && \
    rm -rf /var/lib/apt/lists/*

# Rust, installed system-wide
ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs \
    | sh -s -- -y --default-toolchain nightly --profile minimal --component rust-src \
    && chmod -R a+w /usr/local/rustup /usr/local/cargo

# 3gxtool — built from source, since no prebuilt binary is guaranteed to match this container
RUN git clone --recursive https://gitlab.com/thepixellizeross/3gxtool.git /opt/3gxtool-src \
    && cd /opt/3gxtool-src && mkdir build && cd build \
    && cmake .. && make \
    && install -m 755 3gxtool /usr/local/bin/3gxtool
