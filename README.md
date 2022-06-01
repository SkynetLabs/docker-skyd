# Quick reference

- **Maintained by**:  
  [Skynet Labs](https://skynetlabs.com)

- **Skyd source code and releases**:  
  [SkynetLabs/skyd](https://gitlab.com/SkynetLabs/skyd)

- **Where to get help**:  
  [Skynet Labs Discord](https://discord.gg/skynetlabs) or [Skynet Labs Twitter](https://twitter.com/SkynetLabs)

- **Where to file issues**:  
  [https://github.com/skynetlabs/docker-skyd/issues](https://github.com/skynetlabs/docker-skyd/issues)

# Supported tags and respective `Dockerfile` links

- [`1.5.5`, `1.5.5-scratch`, `1.5`, `1.5-scratch`, `1`, `1-scratch`, `scratch`, `latest`](https://github.com/skynetlabs/docker-skyd/blob/4e8fe34165d79044d7ea909021ccc0de3e3b4c6d/scratch/Dockerfile)
- [`1.5.5-bullseye-slim`, `1.5-bullseye-slim`, `1-bullseye-slim`, `bullseye-slim`](https://github.com/skynetlabs/docker-skyd/blob/4e8fe34165d79044d7ea909021ccc0de3e3b4c6d/bullseye-slim/Dockerfile)

# What is Skynet?

Skynet is a decentralized storage and app hosting platform that makes it easy to join the decentralized internet movement, as a user or a web3 developer.

Skynet apps transform what’s possible on the web. Beyond protecting privacy, decentralization enables application, integration, and innovation that simply cannot be replicated by the centralized world. Now, we can break free of the walled gardens and data silos that have constricted invention and interoperability. Key features of decentralization such as user-owned personal data, persistent identity across apps, and censorship-resistance will be the new standards of the digital world.

> [skynetlabs.com](https://skynetlabs.com)

# How to use this image

See [How To Use This Image](https://github.com/skynetlabs/docker-skyd/blob/main/README.md#how-to-use-this-image) on GitHub for up-to-date documentation.

## Environment variables

All [skyd environment variables](https://gitlab.com/SkynetLabs/skyd/-/raw/master/build/env.go) are supported.

### Default environment variables values

- `SIA_DATA_DIR` defaults to `/sia-data`
- `SIAD_DATA_DIR` defaults to `/sia-data`

⚠️ It is strongly recommended to mount data directories to host filesystem, otherwise all node data will be lost on container shutdown!

### SIA_WALLET_PASSWORD

It is recommended to store your wallet password in `SIA_WALLET_PASSWORD` env variable but make sure you do not commit it to your repository risking exposing it to public.

Initially your wallet password is your seed phrase but you can change it using built in [cli](#cli-client-skyc) `siac wallet change-password` command.

### SIA_MODULES

For webportal usage it is recommended to override `SIA_MODULES` with `gctwra`. It excludes host, miner and exporer modules that are not used by webportal stack.

## CLI client skyc

Every docker-skyd image comes with executable cli client `skyc` that provides information on running `skyd` instance and allows some basic interactions.

Check [documentation](https://gitlab.com/SkynetLabs/skyd/-/blob/master/cmd/skyc/README.md) for available commands.

# Image Variants

The `skyd` images come in many flavours, each designed for a specific use case.

## `skyd:<version>-scratch`

This image is based on the [Scratch](https://hub.docker.com/_/scratch) container designed for publishing super minimal images. It contains only our binaries and whatever they currently require to run properly.

Scratch is the most secure and lightweight image base and we strongly recommend you use this flavour as your defaut image unless you have specific requirements that it cannot satisfy.

Scratch does not include any executable shell or dependency management tool like apt or apk so if you are loooking for an image that allows you to execute commands from within of the container or easily install packages, you should try a different flavour.

## `skyd:<version>-bullseye-slim`

This image is based on the latest [Debian](https://hub.docker.com/_/debian) bullseye-slim image.

Slim is just a lighter version of Debian achieved through removing some extra files that are normally not necessary within containers, such as man pages and documentation.

# License

View [license information](https://gitlab.com/SkynetLabs/skyd/-/blob/master/LICENSE.md) for Skyd or [license information](https://github.com/skyd/docker-skyd/blob/main/LICENSE) for the Skyd Docker project.

As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).

As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.
