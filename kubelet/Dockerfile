FROM docker.io/alpine:3.20 AS fetcher

ARG KUBELET=v1.32.1
ARG ARCH=amd64
ARG SHA=843756e63bc68d46520e81c3e0d476fc13a6c1739a61879bf27895d38e1d276162c9f388f66a474d70ab763142b5b61439805a662ddd448b361cc52d09e7d9e5

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
