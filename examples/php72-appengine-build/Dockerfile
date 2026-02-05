FROM eu.gcr.io/google-appengine/php72
LABEL maintainer="michael@michaeldelle.com"

ENV DEBIAN_FRONTEND=noninteractive \
    CLOUD_SDK_VERSION=344.0.0

RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 8B1DA6120C2BF624; \
    curl -fsSL https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add - \
        && curl -sL https://deb.nodesource.com/setup_10.x | bash -; \
    export DEBIAN_VERSION="$(lsb_release -c -s)"; \
    curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -; \
    echo "deb https://packages.cloud.google.com/apt cloud-sdk-${DEBIAN_VERSION} main" > /etc/apt/sources.list.d/google-cloud-sdk.list; \
    apt-get update && apt-get -y install \
        autoconf \
        bzip2 \
        gcc \
        google-cloud-sdk=${CLOUD_SDK_VERSION}-0 \
        jq \
        kubectl \
        make \
        mysql-client \
        nodejs \
        parallel \
        pkg-config \
        libmcrypt-dev; \
    pecl install mcrypt-1.0.1 \
        redis-4.1.1; \
    echo "extension=gd.so" > /opt/php72/lib/ext.enabled/ext-gd.ini; \
    echo "extension=intl.so" > /opt/php72/lib/ext.enabled/ext-intl.ini; \
    echo "extension=mcrypt.so" > /opt/php72/lib/ext.enabled/ext-mcrypt.ini; \
    echo "extension=redis.so" > /opt/php72/lib/ext.enabled/ext-redis.ini; \
    npm install -g grunt-cli@1.2.0 yarn@1.5.1; \
    apt-get -qy autoremove \
        && apt-get clean \
        && rm -rf /tmp/* /var/lib/apt/lists/*; \
    mkdir -p /tmp/sessions;
