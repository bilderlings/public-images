FROM docker.io/alpine:latest

RUN apk -U --no-cache add dnsmasq curl

WORKDIR /var/lib/tftpboot

RUN curl -sSL -o undionly.kpxe http://boot.ipxe.org/undionly.kpxe && \
    cp undionly.kpxe undionly.kpxe.0 && \
    curl -sSL -o ipxe.efi http://boot.ipxe.org/ipxe.efi && \
    curl -sSL -o snponly-sb.efi https://boot.ipxe.org/x86_64-efi-sb/snponly.efi && \
    curl -sSL -o ipxe-sb.efi https://boot.ipxe.org/x86_64-efi-sb/ipxe.efi

EXPOSE 53/udp 53/tcp 67/udp 69/udp
ENTRYPOINT ["/usr/sbin/dnsmasq", "-k"]
