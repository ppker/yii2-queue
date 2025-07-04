FROM amd64/php:5.6.40-cli-stretch

RUN sed -i s/deb.debian.org/archive.debian.org/g /etc/apt/sources.list \
    && sed -i 's|security.debian.org|archive.debian.org|g' /etc/apt/sources.list \
    && sed -i '/stretch-updates/d' /etc/apt/sources.list

RUN apt-get update -qq \
    && apt install -y --allow-unauthenticated ca-certificates \
    && sed -i '/^mozilla\/DST_Root_CA_X3.crt$/ s/^/!/' /etc/ca-certificates.conf \
    && update-ca-certificates \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update \
    && apt-get install -y --allow-unauthenticated unzip curl zlib1g-dev libicu-dev libpq-dev libgearman-dev

RUN docker-php-ext-install zip pcntl bcmath pdo_mysql intl pdo_pgsql

RUN pecl install igbinary-2.0.8 gearman-1.1.2 \
    && docker-php-ext-enable gearman igbinary

RUN rm -rf /var/lib/apt/lists/*

COPY --from=cytopia/php-cs-fixer:2-php5.6 /usr/bin/php-cs-fixer /usr/local/bin/php-cs-fixer
COPY --from=composer:lts /usr/bin/composer /usr/local/bin/composer

COPY . /code
WORKDIR /code

ENTRYPOINT ["tests/docker/php/entrypoint.sh"]
CMD ["sleep", "infinity"]
