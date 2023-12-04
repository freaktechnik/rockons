# From https://github.com/nextcloud/docker/blob/master/.examples/dockerfiles/full/fpm/Dockerfile
FROM nextcloud:27.1.4-fpm

# ENV PHP_UPLOAD_LIMIT 10G -> nextcloud.ini

# set recommended PHP.ini settings
# see https://docs.nextcloud.com/server/stable/admin_manual/configuration_server/server_tuning.html#enable-php-opcache
# RUN { \
#         echo 'memory_limit=${PHP_MEMORY_LIMIT}'; \
#         echo 'upload_max_filesize=${PHP_UPLOAD_LIMIT}'; \
#         echo 'post_max_size=${PHP_UPLOAD_LIMIT}'; \
#     } > /usr/local/etc/php/conf.d/nextcloud.ini; \
#     \
#     mkdir /var/www/data; \
#     chown -R www-data:root /var/www; \
#     chmod -R g=u /var/www

# Template from https://github.com/nextcloud/docker/blob/master/.examples/dockerfiles/full/fpm/Dockerfile

RUN set -ex; \
    \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        ffmpeg \
        libmagickcore-6.q16-6-extra \
        procps \
        smbclient \
        supervisor \
#       libreoffice \
    ; \
    rm -rf /var/lib/apt/lists/*

RUN set -ex; \
    \
    savedAptMark="$(apt-mark showmanual)"; \
    \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        libbz2-dev \
        libc-client-dev \
        libkrb5-dev \
        libsmbclient-dev \
        libpq-dev \
    ; \
    \
    docker-php-ext-configure imap --with-kerberos --with-imap-ssl; \
    docker-php-ext-install \
        bz2 \
        imap \
        pgsql \
    ; \
    pecl install smbclient; \
    docker-php-ext-enable smbclient; \
    \
# reset apt-mark's "manual" list so that "purge --auto-remove" will remove all build dependencies
    apt-mark auto '.*' > /dev/null; \
    apt-mark manual $savedAptMark; \
    ldd "$(php -r 'echo ini_get("extension_dir");')"/*.so \
        | awk '/=>/ { so = $(NF-1); if (index(so, "/usr/local/") == 1) { next }; gsub("^/(usr/)?", "", so); print so }' \
        | sort -u \
        | xargs -r dpkg-query --search \
        | cut -d: -f1 \
        | sort -u \
        | xargs -rt apt-mark manual; \
    \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
    rm -rf /var/lib/apt/lists/*

# MediaDC https://github.com/andrey18106/mediadc/issues/27, https://github.com/andrey18106/mediadc/wiki/Manual-install

RUN set -ex; \
    \
    apt-get update; \
    apt-get install -y \
        python3-numpy \
        python3-pypillowfight \
        python3-asn1crypto \
        python3-scipy \
        python3-pywt \
        python3-pip \
        python3-distutils \
    ; \
    rm -rf /var/lib/apt/lists/*

RUN set -ex; \
    python3 -m pip install --break-system-packages hexhamming;

# Back to the regularly scheduled fpm enhancements, particularly supervisord

RUN mkdir -p \
    /var/log/supervisord \
    /var/run/supervisord \
;

COPY cron.sh /
COPY supervisord.conf /

RUN chmod +r /supervisord.conf && \
    chmod +x /cron.sh

ENV NEXTCLOUD_UPDATE=1

# Custom (from nextcloud AIO)
RUN set -ex; \
    \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        netcat-openbsd \
        openssl \
        gnupg \
        dirmngr \
        git \
        postgresql-client \
        gnupg \
        tzdata \
        grep \
        nodejs \
        coreutils \
    ; \
    rm -rf /var/lib/apt/lists/*

RUN set -ex; \
    grep -q '^pm = dynamic' /usr/local/etc/php-fpm.d/www.conf; \
    sed -i 's/^pm.max_children =.*/pm.max_children = 100/' /usr/local/etc/php-fpm.d/www.conf; \
    sed -i 's/^pm.start_servers =.*/pm.start_servers = 2/' /usr/local/etc/php-fpm.d/www.conf; \
    sed -i 's/^pm.min_spare_servers =.*/pm.min_spare_servers = 1/' /usr/local/etc/php-fpm.d/www.conf; \
    sed -i 's/^pm.max_spare_servers =.*/pm.max_spare_servers = 3/' /usr/local/etc/php-fpm.d/www.conf

RUN touch /usr/local/etc/php/conf.d/redis-session.ini && \
    chown www-data:www-data /usr/local/etc/php/conf.d/redis-session.ini;

RUN chown www-data:root -R /var/log/supervisord/ && \
    chown www-data:root -R /var/run/supervisord/ && \
    mkdir -p /var/log/nextcloud/ && \
    chown -R www-data:root /var/log/nextcloud/ && \
    chown www-data:root -R /usr/src && \
    chown www-data:www-data -R /usr/local/etc/php/conf.d && \
    chown www-data:root -R /usr/local/etc/php-fpm.d && \
    chmod -R 777 /tmp;

# Give root a random password
RUN echo "root:$(openssl rand -base64 12)" | chpasswd

USER www-data

CMD ["/usr/bin/supervisord", "-c", "/supervisord.conf"]
