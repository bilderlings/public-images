FROM docker.io/alpine:3.21.3 AS fetcher

ARG KUBELET=v1.33.2
ARG ARCH=amd64
ARG SHA=ac478b9504b153cee9d5fea8595621d65380c1040013d2f55070c1fab5a06a035d1e8ca6c62da3f70d8e2a980d7d30765607fde57c6a27c3b42c2de1270cf18c

RUN apk add curl && \
    curl -L https://dl.k8s.io/${KUBELET}/kubernetes-node-linux-${ARCH}.tar.gz -o node.tar.gz && \
    echo "${SHA}  node.tar.gz" | sha512sum -c && \
    tar xzf node.tar.gz kubernetes/node/bin/kubectl kubernetes/node/bin/kubelet


FROM us.gcr.io/k8s-artifacts-prod/build-image/debian-iptables:bullseye-v1.5.5

RUN clean-install --allow-change-held-packages libcap2
RUN clean-install \
    bash \
    ca-certificates \
    ceph-common \
    cifs-utils \
    e2fsprogs \
    xfsprogs \
    ethtool \
    glusterfs-client \
    iproute2 \
    jq \
    nfs-common \
    socat \
    udev \
    util-linux

COPY --from=fetcher /kubernetes/node/bin/kubelet /usr/local/bin/kubelet
COPY --from=fetcher /kubernetes/node/bin/kubectl /usr/local/bin/kubectl
ENTRYPOINT ["/usr/local/bin/kubelet"]
