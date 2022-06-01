FROM golang:1.18.2-bullseye AS builder

LABEL maintainer="Skynet Labs <devs@skynetlabs.com>"

WORKDIR /root

ENV CGO_ENABLED 0
ENV GOOS linux
ENV GOARCH amd64

ARG branch

RUN git clone https://gitlab.com/SkynetLabs/skyd.git --single-branch --depth 1 --branch ${branch} && \
    make build --directory skyd

# RUN wget -q https://gitlab.com/SkynetLabs/skyd/-/archive/${branch}/skyd-${branch}.tar.gz && \
#     tar -xvzf skyd-${branch}.tar.gz --directory skyd && \
#     make build --directory skyd

FROM scratch

LABEL maintainer="Skynet Labs <devs@skynetlabs.com>"

# copy mime types
COPY --from=builder /etc/mime.types /etc/mime.types

# copy skyd and skyc binaries
COPY --from=builder /go/bin/ /root/skyd/bin

ENV SIA_DATA_DIR /sia-data
ENV SIAD_DATA_DIR /sia-data

ENTRYPOINT [ "skyd" ]
