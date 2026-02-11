#  Private Magic Wormhole server

To start your own Magic Wormhole server, you need to install and run both a
Mailbox Server and a Transit Relay.

The command line tool (wormhole) is preconfigured to use public servers, so
running your own is for custom or production environments. 

<!-- toc -->
- [Running the Servers](#running-the-servers)
- [Private Wormhole Relay Installation](#private-wormhole-relay-installation)
- [Neovim Plugins](#neovim-plugins)
- [Build Teamtype (MSYS2)](#build-teamtype-msys2)
- [Wireguard](#wireguard)
- [Annexes](#annexes)
<!-- /toc -->

## Running the Servers

First activate venv (check `.venv/bin` for the appropriate script to source)

Start mailbox server

    twist wormhole-mailbox --usage-db=usage.sqlite --port=tcp:4000

Start transit relay

    twist transitrelay --port=tcp:4001

## Private Wormhole Relay Installation

Create venv

    uv venv

Activate env

    source .venv/bin/activate.fish # or follow instruction

Install requirements

    uv pip install magic-wormhole-mailbox-server magic-wormhole-transit-relay

## Neovim Plugins

Adapt the plugin‑configuration examples that use <lazy.nvim> so they work with
whichever package manager you prefer.

The **[pair‑ls](https://github.com/stevearc/pair-ls.nvim)** plugin is no longer
maintained. It was designed to supply observers for collaborative work, but
tunneling isn’t handled by any third‑party service and must be set up manually
(e.g. see [wireguard section](#wireguard)).

The **[live-share](https://github.com/azratul/live-share.nvim)** plugin heavily relies on reverse tunneling services like <https://serveo.net> and <https://localhost.run>

## Build Teamtype (MSYS2)

<https://github.com/rust-lang/rust/blob/main/INSTALL.md#building-on-windows/>

    pacman -S make \
                diffutils \
                tar \
                mingw-w64-x86_64-python \
                mingw-w64-x86_64-cmake \
                mingw-w64-x86_64-gcc \
                mingw-w64-x86_64-ninja

    pacman -S mingw-w64-clang-x86_64-rust mingw-w64-clang-x86_64-clang

    $USERPROFILE/.cargo/bin/rustup target add x86_64-pc-windows-msvc
    $USERPROFILE/.cargo/bin/rustup target add x86_64-pc-windows-gnu

    $USERPROFILE/.cargo/bin/rustc --print=cfg


    $USERPROFILE/.cargo/bin/rg --files -g gcc.exe /

    PATH=$PATH:/mingw64/bin \
    $USERPROFILE/.cargo/bin/cargo install teamtype --target=x86_64-pc-windows-gnu

    # $USERPROFILE/.cargo/config
    [build]
    target = ["x86_64-unknown-linux-gnu", "i686-unknown-linux-gnu"]

## Wireguard

<https://gist.github.com/malikbenkirane/f9ed53dac24267fc660b6810d85faba7>

## Annexes

- <https://github.com/redeltaglio>
- <https://github.com/redeltaglio/altBSD_network>
- <https://docs.cloud.google.com/confidential-computing/confidential-vm/docs/confidential-vm-overview>
- <https://cloud.google.com/compute/vm-instance-pricing>
- <https://cloud.google.com/compute/all-pricing>
- <https://cloud.google.com/confidential-computing/confidential-vm/pricing>
