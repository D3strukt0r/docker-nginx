ARG NGINX_VERSION=latest

FROM nginx:${NGINX_VERSION}

# Platform: linux/amd64,linux/arm64

LABEL org.opencontainers.image.authors="D3strukt0r <dev@d3strukt0r.me>" \
      org.opencontainers.image.created=${{ steps.prep.outputs.created }} \
      org.opencontainers.image.url=${{ github.event.repository.html_url }} \
      org.opencontainers.image.source=${{ github.event.repository.clone_url }} \
      org.opencontainers.image.version=${{ steps.prep.outputs.version }} \
      org.opencontainers.image.revision=${{ github.sha }} \
      org.opencontainers.image.title=${{ github.event.repository.name }} \
      org.opencontainers.image.description=${{ github.event.repository.description }} \
      org.opencontainers.image.licenses=${{ github.event.repository.license.spdx_id }} \

SHELL ["/bin/bash", "-e", "-u", "-o", "pipefail", "-c"]

RUN <<EOT
    #!/bin/bash -e -u -x -o pipefail
    apt-get update
    apt-get install --no-install-recommends --no-install-suggests --yes \
        bash-completion \
        wget \
        gettext
    rm -rf /var/lib/apt/lists/*

    # Setup a red prompt for root and a green one for users.
    cat <<EOT >> ~/.bashrc
    NORMAL="\e[0m"
    GREEN="\e[38;5;10m"
    RED="\e[1;31m"
    BLUE="\e[38;5;12m"

    if [ "$USER" = root ]; then
        PS1="🐳 $RED\u@\h$NORMAL:$BLUE\w$NORMAL\\$ "
    else
        PS1="🐳 $GREEN\u@\h$NORMAL:$BLUE\w$NORMAL\\$ "
    fi
    EOT

    # Fix no www folder
    mkdir --parents /var/www
    chown --changes --silent --no-dereference --recursive www-data:www-data \
        /var/www
EOT
