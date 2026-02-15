Private Magic Wormhole server
=============================

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

Running the Servers
----------------------

First activate venv (check `.venv/bin` for the appropriate script to source)

Start mailbox server

    twist wormhole-mailbox --usage-db=usage.sqlite --port=tcp:4000

Start transit relay

    twist transitrelay --port=tcp:4001

Finally

    teamtype share --magic-wormhole-relay ws://10.20.10.2:4000/v1

Private Wormhole Relay Installation
--------------------------------------

Since the PyPI package is outdated, you can use the repository directly:

### Prerequisites

[uv installation](https://github.com/malikbenkirane/yt-live/tree/main/wormhole#private-wormhole-relay-installation)

```bash
uv pip install setuptools magic-wormhole
```

### Install the mailbox server  

```bash
# 1. Install the required tools
uv pip install --upgrade pip setuptools          # upgrades pip and installs setuptools

# 2. Clone the repository (shallow clone to save bandwidth)
git clone --depth=1 https://github.com/magic-wormhole/magic-wormhole-mailbox-server
cd magic-wormhole-mailbox-server

# 3. Create an isolated virtual environment with uv
uv venv .venv          # creates .venv inside the project directory
source .venv/bin/activate   # activate it (use `.venv\Scripts\activate` on Windows)

# 4. Install the server package
python setup.py install

# 5. Install the client library (so you can talk to the server)
uv pip install magic-wormhole
```

### Install the transit‑relay server  

```bash
# 1. Install the required tools (if not already done)
uv pip install --upgrade pip setuptools

# 2. Clone the transit‑relay repository
git clone --depth=1 https://github.com/magic-wormhole/magic-wormhole-transit-relay
cd magic-wormhole-transit-relay

# 3. Create a virtual environment
uv venv .venv
source .venv/bin/activate   # on Windows: `.venv\Scripts\activate`

# 4. Install the relay package
python setup.py install

# 5. Install the client library (same as for the mailbox server)
uv pip install magic-wormhole
```

**Tips**

- Run `deactivate` to leave the virtual environment when you’re done.  
- Use `uv pip list` inside the venv to verify that `magic-wormhole` (and any other dependencies) are installed.  

**Using PyPI**

1. **Create a virtual environment**  

   ```bash
   uv venv
   ```

2. **Activate the environment**  

   ```bash
   source .venv/bin/activate.fish   # or use the activation command appropriate for your shell
   ```

3. **Install the required packages**  

   ```bash
   uv pip install magic-wormhole-mailbox-server magic-wormhole-transit-relay
   ```

Neovim Plugins
--------------

Adapt the plugin‑configuration examples that use <lazy.nvim> so they work with
whichever package manager you prefer.

The **[pair‑ls](https://github.com/stevearc/pair-ls.nvim)** plugin is no longer
maintained. It was designed to supply observers for collaborative work, but
tunneling isn’t handled by any third‑party service and must be set up manually
(e.g. see [wireguard section](#wireguard)).

The **[live‑share](https://github.com/azratul/live-share.nvim)** plugin depends
heavily on reverse‑tunneling services such as <https://serveo.net> and
<https://localhost.run>, and it does not provide support for private relays.

The **[teamtype-nvim](https://github.com/teamtype/teamtype-nvim)** meant to be
used with Teamtype, but can also be configured to work with other collaborative
software speaking the same protocol.

TeamType
--------

### Nvim Plugin

<https://github.com/teamtype/teamtype-nvim>

### Private Relay

    teamtype share --magic-wormhole-relay RELAY_URL

### Installation

    brew install teamtype

### Build Teamtype (MSYS2)

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

Wireguard
---------

<https://gist.github.com/malikbenkirane/f9ed53dac24267fc660b6810d85faba7>

Annexes
-------

- <https://github.com/redeltaglio>
- <https://github.com/redeltaglio/altBSD_network>
- <https://docs.cloud.google.com/confidential-computing/confidential-vm/docs/confidential-vm-overview>
- <https://cloud.google.com/compute/vm-instance-pricing>
- <https://cloud.google.com/compute/all-pricing>
- <https://cloud.google.com/confidential-computing/confidential-vm/pricing>

Troubleshooting
---------------

    rm *.sqlite
    twist wormhole-mailbox --usage-db=usage.sqlite --port=tcp:4000
    twist transitrelay --port=tcp:4001

    rm -rf .teamtype
    # then restart magic wormhole

Issues may occur with too many files... 
