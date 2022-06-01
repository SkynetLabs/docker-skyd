FROM golang:1.18.2-bullseye AS builder

LABEL maintainer="Skynet Labs <devs@skynetlabs.com>"

WORKDIR /root

ENV CGO_ENABLED 0
ENV GOOS linux
ENV GOARCH amd64

ARG branch

RUN git clone https://gitlab.com/SkynetLabs/skyd.git --single-branch --depth 1 --branch ${branch} && \
    make release --directory skyd && \
    # when cross compiling, go installs binaries into subdirectory and we need to copy them over
    if [ "${GOOS}" != "$(go env GOHOSTOS)" ] || [ "${GOARCH}" != "$(go env GOHOSTARCH)" ]; then \
        cp /go/bin/${GOOS}_${GOARCH}/* /go/bin; \
    fi

# RUN wget -q https://gitlab.com/SkynetLabs/skyd/-/archive/${branch}/skyd-${branch}.tar.gz && \
#     tar -xvzf skyd-${branch}.tar.gz && \
#     make release --directory skyd-${branch}

FROM debian:bullseye-slim

LABEL maintainer="Skynet Labs <devs@skynetlabs.com>"

# copy mime types
COPY --from=builder /etc/mime.types /etc/mime.types

# copy skyd and skyc binaries
COPY --from=builder /go/bin/ /usr/bin

ENV SIA_DIR /sia
ENV SIA_DATA_DIR /sia-data
ENV SIAD_DATA_DIR /sia-data

ENTRYPOINT [ "skyd" ]
