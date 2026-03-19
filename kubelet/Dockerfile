FROM docker.io/alpine:3 AS fetcher

ARG KUBELET=v1.35.2
ARG ARCH=amd64
ARG SHA=499a61b38dbcddb7105a89cac0f63e1d9a08178c051d693a8095de65939deb4e12ff4a11116d69ec69e14da0d929c8344bbb8ea8ad06cc65d749d327ee0c7c55

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
