FROM docker.io/alpine:3 AS fetcher

ARG KUBELET=v1.34.3
ARG ARCH=amd64
ARG SHA=fe7adba29237038eb86e67610df515e7c285b2a399c9d4587fba182e2ce302a93de20a283f76e5d5fa1ba81eb1f735a8f3f1b264071bb4154c9d51d884cf69240b8f1183763d3db73bacac699946268d9aac462fb730a3760ed72fc9318b8df7e1ef3b150e12fc2ff42dfeb13c4ffca80b5ad38eee0b3bb03522ba972f3119dd

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
