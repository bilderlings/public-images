FROM docker.io/alpine:3.21.3 AS fetcher

ARG KUBELET=v1.33.3
ARG ARCH=amd64
ARG SHA=90d5aa5c08d01febea7f2afe11fb7771568494e68c5cf7b2c1a245b9de24d7962e207efa218ecba45540a2f613b13cf561a8b5f5618f9422042f40a8d7e88988

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
