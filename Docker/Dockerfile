# Build the desktop rucio container
# See the Start up scripts to see how to run this container.
# For more info see the documentation.
ARG RUCIO_VERSION=release-1.19.8
FROM rucio/rucio-clients:${RUCIO_VERSION}

# Metadata
LABEL maintainer="gwatts@uw.edu"

# The certificates so rucio can properly access everything
COPY certs/opt/rucio/etc/rucio.cfg /opt/rucio/etc/rucio.cfg
COPY certs/etc/ca.crt /etc/ca.crt

COPY certs/etc/vomses/ /etc/vomses/
COPY certs/etc/grid-security/certificates/ /etc/grid-security/certificates/
COPY certs/etc/grid-security/vomsdir/ /etc/grid-security/vomsdir/

# Setup for default location for certs and rucio running
RUN mkdir /root/.globus
ENV X509_USER_PROXY=/tmp/grid-proxy
ENV RUCIO_HOME=

WORKDIR /root
ENTRYPOINT ["/bin/bash"]
