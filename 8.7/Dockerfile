# Docker image TYPO3 CMS 8.7
# (c) 2024 Kuno Sandholzer <sandholzer@vobs.at>
# basierend auf der Arbeit von Martin Helmich <martin@helmich.me>
#                               https://github.com/martin-helmich/docker-typo3/
#
# Dieses Docker Image ist an die speziellen Beduerfnisse auf vobs.at angepasst.
#
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <https://www.gnu.org/licenses/>.
#

FROM php:7.4-apache-buster
LABEL maintainer="Kuno Sandholzer <sandholzer@vobs.at>"

# Install TYPO3
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
        wget \
# Configure PHP
        libxml2-dev libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libpq-dev \
        libzip-dev \
        zlib1g-dev \
# Install required 3rd party tools
        graphicsmagick && \
# Configure extensions
    docker-php-ext-configure gd --with-libdir=/usr/include/ --with-jpeg --with-freetype && \
    docker-php-ext-install -j$(nproc) mysqli soap gd zip opcache intl pgsql pdo_pgsql && \
    echo 'always_populate_raw_post_data = -1\nmax_execution_time = 240\nmax_input_vars = 1500\nupload_max_filesize = 32M\npost_max_size = 32M' > /usr/local/etc/php/conf.d/typo3.ini && \
# Configure Apache as needed
    a2enmod rewrite && \
    apt-get clean && \
    apt-get -y purge \
        libxml2-dev libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libzip-dev \
        zlib1g-dev && \
    rm -rf /var/lib/apt/lists/* /usr/src/*

RUN cd /var/www/html && \
    wget -O download.tar.gz https://get.typo3.org/8.7.32 && \
    echo "07b49f127bce5e05c930819168142b091f2dbf38c33adb6caf7e10d65e4fe263 download.tar.gz" > download.tar.gz.sum && \
    sha256sum -c download.tar.gz.sum && \
    tar -xzf download.tar.gz && \
    rm download.* && \
    ln -s typo3_src-* typo3_src && \
    ln -s typo3_src/index.php && \
    ln -s typo3_src/typo3 && \
    ln -s typo3_src/_.htaccess .htaccess && \
    mkdir typo3temp && \
    mkdir typo3conf && \
    mkdir fileadmin && \
    mkdir uploads && \
    touch FIRST_INSTALL && \
    chown -R www-data. .

# Configure volumes
VOLUME /var/www/html/fileadmin
VOLUME /var/www/html/typo3conf
VOLUME /var/www/html/typo3temp
VOLUME /var/www/html/uploads


# Add VOBS Typo3 PHP settings
COPY ./vobs-typo3-custom-php.ini /usr/local/etc/php/conf.d/vobs-typo3-custom-php.ini

# Install GhostScript, remove temporary apt files and remove typo3-Installation by martinhelmich/typo3

RUN apt-get update -o Acquire::Check-Valid-Until=false && \
    apt-get install -y --no-install-recommends ghostscript-x graphicsmagick-imagemagick-compat
RUN rm -rf /var/lib/apt/lists/* /usr/src/* && \
    rm -rf /var/www/html/typo3_src-*

