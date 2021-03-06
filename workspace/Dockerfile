#
#--------------------------------------------------------------------------
# Image Setup
#--------------------------------------------------------------------------
#

FROM laradock/workspace:1.0

MAINTAINER Mahmoud Zalt <mahmoud@zalt.me>

#
#--------------------------------------------------------------------------
# Mandatory Software's Installation
#--------------------------------------------------------------------------
#
# Mandatory Software's such as ("php7.0-cli", "git", "vim", ....) are
# installed on the base image 'laradock/workspace' image. If you want
# to add more Software's or remove existing one, you need to edit the
# base image (https://github.com/LaraDock/workspace).
#

#
#--------------------------------------------------------------------------
# Optional Software's Installation
#--------------------------------------------------------------------------
#
# Optional Software's will only be installed if you set them to `true`
# in the `docker-compose.yml` before the build.
#
#   - INSTALL_XDEBUG=           false
#   - INSTALL_MONGO=            false
#   - COMPOSER_GLOBAL_INSTALL=  false
#   - INSTALL_NODE=             false
#

#####################################
# xDebug:
#####################################

# Check if xDebug needs to be installed
ARG INSTALL_XDEBUG=true
ENV INSTALL_XDEBUG ${INSTALL_XDEBUG}
RUN if [ ${INSTALL_XDEBUG} = true ]; then \
    # Load the xdebug extension only with phpunit commands
    apt-get install -y --force-yes php7.0-xdebug && \
    sed -i 's/^/;/g' /etc/php/7.0/cli/conf.d/20-xdebug.ini && \
    echo "alias phpunit='php -dzend_extension=xdebug.so /var/www/laravel/vendor/bin/phpunit'" >> ~/.bashrc \
;fi

#####################################
# MongoDB:
#####################################

# Check if Mongo needs to be installed
ARG INSTALL_MONGO=true
ENV INSTALL_MONGO ${INSTALL_MONGO}
RUN if [ ${INSTALL_MONGO} = true ]; then \
    # Install the mongodb extension
    pecl install mongodb && \
    echo "extension=mongodb.so" >> /etc/php/7.0/cli/php.ini \
;fi

#####################################
# Composer:
#####################################

# Add the composer.json
ADD ./composer.json /root/.composer/composer.json

# Check if global install need to be runned
ARG COMPOSER_GLOBAL_INSTALL=true
ENV COMPOSER_GLOBAL_INSTALL ${COMPOSER_GLOBAL_INSTALL}
RUN if [ ${COMPOSER_GLOBAL_INSTALL} = true ]; then \
    # run the install
    composer global install \
;fi

#####################################
# Node / NVM:
#####################################

# Check if NVM needs to be installed
ARG INSTALL_NODE=true
ENV INSTALL_NODE ${INSTALL_NODE}
RUN if [ ${INSTALL_NODE} = true ]; then \
    # Install nvm (A Node Version Manager)
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.1/install.sh | bash \
;fi
# Again check if NVM needs to be installed
# I had to split this condifiton link this because when I get it inside the above if statment is refuses to work!
ENV if [ ${INSTALL_NODE} = true ]; then \
    # Set the ENV
    NVM_DIR=/root/.nvm \
    # Install NodeJS with NVM
    RUN . ~/.nvm/nvm.sh && \
        nvm install stable && \
        nvm use stable && \
        nvm alias stable && \
        npm install -g gulp bower \
;fi

#
#--------------------------------------------------------------------------
# Final Touch
#--------------------------------------------------------------------------
#

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Set default work directory
WORKDIR /var/www/laravel
