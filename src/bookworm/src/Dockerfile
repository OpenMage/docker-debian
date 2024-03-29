FROM debian:bookworm-slim

SHELL ["/bin/bash", "-o", "pipefail", "-c"]
ENV BASH_ENV=/etc/profile \
    DEBIAN_FRONTEND=noninteractive \
    PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
WORKDIR /tmp/

ARG CONTAINER_RUNTIME_REQUIREMENTS="less procps netcat-traditional ca-certificates iputils-ping iproute2 unzip"
ARG CONTAINER_RUNTIME_REQUIREMENTS_EXTRA=""
ARG CONTAINER_BUILD_REQUIREMENTS=""
ARG CONTAINER_BUILD_REQUIREMENTS_EXTRA=""

COPY root /

# hadolint ignore=DL3008,DL3015
RUN set -Eeu; \
    chmod 755 /usr/local/bin/{docker-layer-clean,docker-install-requirements,docker-package-download}; \
    echo "APT::Install-Recommends \"false\";" > /etc/apt/apt.conf.d/docker-disable-recommends; \
    echo "export PATH=\"${PATH}\"" > /etc/environment; \
    mkdir -p /usr/share/man/man1; \
    mkdir -p /usr/share/man/man7; \
    /usr/local/bin/docker-install-requirements; \
    sed -i '/^mozilla\/DST_Root_CA_X3/s/^/!/' /etc/ca-certificates.conf; \
    update-ca-certificates -f; \
    /usr/local/bin/docker-layer-clean
