FROM alpine:3.18

RUN apk add --no-cache bash yq jq curl
RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.24.3/bin/linux/amd64/kubectl
RUN chmod +x kubectl
RUN mv kubectl /bin/kubectl

USER 65534 : 65534
