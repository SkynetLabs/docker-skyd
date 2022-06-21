FROM golang:1.18.3-bullseye AS builder

LABEL maintainer="Skynet Labs <devs@skynetlabs.com>"

WORKDIR /root

ENV CGO_ENABLED 0
ENV GOOS linux
ENV GOARCH amd64

ARG branch

RUN git clone https://gitlab.com/SkynetLabs/skyd.git --single-branch --depth 1 --branch ${branch} && \
    make build --directory skyd

FROM debian:bullseye-slim

LABEL maintainer="Skynet Labs <devs@skynetlabs.com>"

# copy mime types
COPY --from=builder /etc/mime.types /etc/mime.types

# copy skyd and skyc binaries
COPY --from=builder /root/skyd/bin/ /usr/bin

# backwards compatibility with older versions of sia docker image
# note: even though we could create a symlink, the default scratch 
# image does not support them and to maintain full sync between images
# it is safer to just copy the binary like scratch image does
COPY --from=builder /root/skyd/bin/skyc /usr/bin/siac

ENV SIA_DATA_DIR /sia-data
ENV SIAD_DATA_DIR /sia-data

ENTRYPOINT [ "skyd" ]
