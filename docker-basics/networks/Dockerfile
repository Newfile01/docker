FROM alpine:3.24

WORKDIR /
RUN apk add --no-cache \
    iputils \
    bind-tools \
    traceroute \
    curl \
    iproute2 \
    busybox-extras

SHELL ["/bin/bash", "-c"]

CMD [ "sleep", "infinity"]