FROM ghcr.io/home-assistant/base:latest

# Install python + pip
WORKDIR /srv
RUN \
  apk add --no-cache \
    python3 py3-pip py3-yaml

# Prepare venv
RUN cd /srv && python3 -m venv venv

# Install modbus + py3yaml via pip
RUN cd /srv && source ./venv/bin/activate && \
    pip install modbus-proxy PyYAML

# PATCH: vervang het commando modbus-proxy door de gepatchte versie
COPY modbus_proxy_patched.py /srv/modbus_proxy_patched.py
RUN printf '#!/bin/sh\nexec /srv/venv/bin/python /srv/modbus_proxy_patched.py "$@"\n' > /srv/venv/bin/modbus-proxy && \
    chmod +x /srv/venv/bin/modbus-proxy

# Copy data for add-on
WORKDIR /
COPY run.sh /
WORKDIR /srv

RUN chmod a+x /run.sh
CMD [ "/run.sh" ]
