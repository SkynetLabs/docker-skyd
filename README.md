[![View on Dockerhub](https://img.shields.io/badge/view%20on-dockerhub-blue)](https://hub.docker.com/repository/docker/skynetlabs/skyd)
[![Docker Image Version (latest semver)](https://img.shields.io/docker/v/skynetlabs/skyd?color=00c65e&sort=semver)]()
[![Docker Image Size (latest semver)](https://img.shields.io/docker/image-size/skynetlabs/skyd?color=00c65e&sort=semver)]()
[![Docker Pulls](https://img.shields.io/docker/pulls/skynetlabs/skyd?color=00c65e)]()

# Quick reference

- **Maintained by**:  
  [Skynet Labs](https://skynetlabs.com)

- **Skyd source code and releases**:  
  [SkynetLabs/skyd](https://gitlab.com/SkynetLabs/skyd)

- **Skyd license information**:  
  [Skynet License 1.0](https://gitlab.com/SkynetLabs/skyd/-/blob/master/LICENSE.md)

- **Where to get help**:  
  [Skynet Labs Discord](https://discord.gg/skynetlabs) or [Skynet Labs Twitter](https://twitter.com/SkynetLabs)

- **Where to file issues**:  
  [https://github.com/skynetlabs/docker-skyd/issues](https://github.com/skynetlabs/docker-skyd/issues)

# Supported tags and respective `Dockerfile` links

- [`1.6.2-scratch`, `1.6-scratch`, `1-scratch`, `scratch`, `latest`](https://github.com/SkynetLabs/docker-skyd/blob/v1.6.2/scratch/Dockerfile)
- [`1.6.2-bullseye-slim`, `1.6-bullseye-slim`, `1-bullseye-slim`, `bullseye-slim`](https://github.com/SkynetLabs/docker-skyd/blob/v1.6.2/bullseye-slim/Dockerfile)

# What is Skynet?

Skynet is a decentralized storage and app hosting platform that makes it easy to join the decentralized internet movement, as a user or a web3 developer.

Skynet apps transform what’s possible on the web. Beyond protecting privacy, decentralization enables application, integration, and innovation that simply cannot be replicated by the centralized world. Now, we can break free of the walled gardens and data silos that have constricted invention and interoperability. Key features of decentralization such as user-owned personal data, persistent identity across apps, and censorship-resistance will be the new standards of the digital world.

> [skynetlabs.com](https://skynetlabs.com)

# How to use this image

<!-- this direct link is here because dockerhub might have outdated version of readme on repository page -->

See [How To Use This Image](https://github.com/skynetlabs/docker-skyd/blob/main/README.md#how-to-use-this-image) on GitHub for up-to-date documentation.

### Startup command

This image declares `ENTRYPOINT [ "skyd" ]` and does not override any default command line arguments. You are expected to set those yourself as a part of the command.

#### Webportal stack required arguments

When running skyd image as a part of the skynet webportal stack, you need to change enabled modules and make the api accessible.

```yaml
services:
  skyd:
    image: skynetlabs/skyd
    command: --disable-api-security --api-addr :9980 --modules gtcwra
```

## Environment variables

All [skyd environment variables](https://gitlab.com/SkynetLabs/skyd/-/raw/master/build/env.go) are supported.

### Default environment variables values

- `SIA_DATA_DIR` defaults to `/sia-data`
- `SIAD_DATA_DIR` defaults to `/sia-data`

⚠️ It is strongly recommended to mount data directories to host filesystem, otherwise all node data will be lost on container shutdown!

### SIA_WALLET_PASSWORD

It is recommended to store your wallet password in `SIA_WALLET_PASSWORD` env variable but make sure you do not commit it to your repository risking exposing it to public. If this env variable is not set then skyd will require you to unlock the wallet each time it gets restarted.

Initially your wallet password is your seed phrase but you can change it using built in [cli](#cli-client-skyc) command `skyc wallet change-password`.

## Logs rotation

Skyd produces multiple log files that are persisted to disk in append mode. These log files can grow significantly and it is recommended to rotate them based on size of the files.

Easiest way to set up log rotation is to either configure [logrotate](https://linux.die.net/man/8/logrotate) on host machine or use [blacklabelops/logrotate](https://hub.docker.com/r/blacklabelops/logrotate) image.

Example logrotate configuration (change `/sia-data` to the directory that mounts it):

```
"/sia-data/*.log" "/sia-data/*/*.log" "/sia-data/*/*/*.log" {
    size 100M
    rotate 10
    compress
    dateext
    copytruncate
}
```

## CLI client skyc

Every docker-skyd image comes with executable cli client `skyc` that provides information on running `skyd` instance and allows some basic interactions.

Check [documentation](https://gitlab.com/SkynetLabs/skyd/-/blob/master/cmd/skyc/README.md) for available commands.

# Image Variants

The `skyd` images come in two flavors, each designed for a specific use case.

## `skyd:<version>-scratch`

This image is based on the [Scratch](https://hub.docker.com/_/scratch) container designed for publishing super minimal images. It contains only our binaries and whatever they currently require to run properly.

Scratch is the most secure and lightweight image base and we strongly recommend you use this flavor as your defaut image unless you have specific requirements that it cannot satisfy.

Scratch does not include any executable shell or dependency management tool like apt or apk so if you are loooking for an image that allows you to execute commands from within of the container or easily install packages, you should try a different flavor.

## `skyd:<version>-bullseye-slim`

This image is based on the latest [Debian](https://hub.docker.com/_/debian) bullseye-slim image.

Slim is just a lighter version of Debian achieved through removing some extra files that are normally not necessary within containers, such as man pages and documentation.

# License

View [license information](https://gitlab.com/SkynetLabs/skyd/-/blob/master/LICENSE.md) for Skyd or [license information](https://github.com/skyd/docker-skyd/blob/main/LICENSE) for the Skyd Docker project.

As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).

As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.
