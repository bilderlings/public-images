FROM docker.io/alpine:3 AS fetcher

ARG KUBELET=v1.35.3
ARG ARCH=amd64
ARG SHA=0968dbee07458123ed6482497f9cec6659f365a55192713635fb7d7b07904d41f16fb376e2a47ea5375d0b77e5fdf519738e1b32b8eab799054280a3ee24c018

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
