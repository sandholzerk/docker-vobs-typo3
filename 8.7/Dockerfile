# Docker image TYPO3 CMS 10.4
# (c) 2024 Kuno Sandholzer <webmaster@vobs.at|
#
#

FROM martinhelmich/typo3:8.7
LABEL maintainer="Kuno Sandholzer <webmaster@vobs.at>"

# Prepare APT to work with cnv.at
#COPY ./apt-files/trusted.gpg /etc/apt
#COPY ./apt-files/sources.list.d/* /etc/apt/sources.list.d
#RUN rm /etc/apt/sources.list.d/debian.sources

# Add VOBS Typo3 PHP settings
COPY ./vobs-typo3-custom-php.ini /usr/local/etc/php/conf.d/vobs-typo3-custom-php.ini

# Install GhostScript, remove temporary apt files and remove typo3-Installation by martinhelmich/typo3:11.5

RUN apt-get update -o Acquire::Check-Valid-Until=false && \
    apt-get install -y --no-install-recommends ghostscript-x graphicsmagick-imagemagick-compat
RUN rm -rf /var/lib/apt/lists/* /usr/src/* && \
    rm -rf /var/www/html/typo3_src-*

