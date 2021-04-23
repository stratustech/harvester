FROM alpine
RUN apk update && apk add -u --no-cache git curl unzip tar tini bash nfs-utils && \
    adduser -D harvester && su -l harvester && \
    mkdir -p /var/lib/rancher/harvester && \
    chown -R harvester /var/lib/rancher/harvester /usr/local/bin

WORKDIR /var/lib/rancher/harvester

ENV HARVESTER_UI_VERSION v0.2.0-rc3
ENV HARVESTER_UI_PATH /usr/share/rancher/harvester
# Please update the api-ui-version in pkg/settings/settings.go when updating the version here.
ENV HARVESTER_API_UI_VERSION 1.1.9

ARG VERSION=dev
ENV HARVESTER_SERVER_VERSION ${VERSION}
RUN mkdir -p /usr/share/rancher/harvester && \
    mkdir -p /usr/share/rancher/harvester/api-ui

COPY ${HARVESTER_UI_VERSION}.tar.gz /usr/share/rancher/harvester/
COPY ${HARVESTER_API_UI_VERSION}.tar.gz /usr/share/rancher/harvester/api-ui
RUN cd /usr/share/rancher/harvester && tar xvzf ${HARVESTER_UI_VERSION}.tar.gz --strip-components=2
RUN cd /usr/share/rancher/harvester/api-ui && tar xvzf ${HARVESTER_API_UI_VERSION}.tar.gz --strip-components=1
RUN cd /var/lib/rancher/harvester

COPY entrypoint.sh harvester /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh

VOLUME /var/lib/rancher/harvester
ENTRYPOINT ["entrypoint.sh"]
