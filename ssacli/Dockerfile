FROM debian:bullseye

ENV DEBIAN_FRONTEND noninteractive

RUN apt update; \
    apt install -y wget procps gnupg2; \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys C208ADDE26C2B797; \
    echo "deb http://downloads.linux.hpe.com/SDR/repo/mcp bullseye/current non-free" > /etc/apt/sources.list.d/hp-mcp.list; \
    apt update; \
    apt install -y ssacli

COPY ./docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
