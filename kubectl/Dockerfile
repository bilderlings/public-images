FROM alpine:3.21.3

RUN apk add --no-cache bash yq jq curl
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
RUN chmod +x kubectl
RUN mv kubectl /bin/kubectl

USER 65534 : 65534
