FROM docker.io/alpine:3.18 AS fetcher

ARG KUBELET=v1.29.2
ARG ARCH=amd64
ARG SHA=7b4ae81efc8a8bf0ece659c83dc6de0d4730b97d917e6b4c9f15fba9155422ee847bbd4464e241b9689afcda80ff2753d73e9663de167947f81084fac15b6e66

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
