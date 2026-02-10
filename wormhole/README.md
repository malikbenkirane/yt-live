#  Private Magic Wormhole server

To start your own Magic Wormhole server, you need to install and run both a
Mailbox Server and a Transit Relay.

The command line tool (wormhole) is preconfigured to use public servers, so
running your own is for custom or production environments. 

<!-- toc -->
- [Running the Servers](#running-the-servers)
- [Private Wormhole Relay Installation](#private-wormhole-relay-installation)
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

