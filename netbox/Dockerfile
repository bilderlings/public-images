FROM docker.io/netboxcommunity/netbox:v4.7-5.1.0

USER root

COPY requirements.txt /tmp/requirements.txt

RUN /usr/local/bin/uv pip install \
    --python /opt/netbox/venv/bin/python \
    --no-cache \
    -r /tmp/requirements.txt

USER netbox
