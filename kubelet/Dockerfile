FROM docker.io/alpine:3.20 AS fetcher

ARG KUBELET=v1.32.2
ARG ARCH=amd64
ARG SHA=92df813a32e157827c69c8c5c4843c6a994d7a52750ae5d3b06d136bd2d61386a55a878a425f4e29f10a9de56c0638d49d34c7b96c8cf391924c76e225ed78bb

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
