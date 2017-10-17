FROM php:7.1.10-fpm-jessie

ENV H2O_BRANCH v2.2.x
# v2.2.2
ENV H2O_COMMITISH 7677fce9e41668c6dc21928a19f7d977a186b4c4

RUN apt-get -qy update
RUN apt-get -qy install git cmake
RUN mkdir -p /tmp/h2o
RUN git clone -b $H2O_BRANCH --depth=50 https://github.com/h2o/h2o.git /tmp/h2o/h2o

RUN apt-get -qy install --no-install-recommends \
        bzip2 \
        file \
        g++ \
        gcc \
        libbz2-dev \
        libc6-dev \
        libcurl4-openssl-dev \
        libdb-dev \
        libevent-dev \
        libffi-dev \
        libglib2.0-dev \
        libkrb5-dev \
        libncurses-dev \
        libpng-dev \
        libreadline-dev \
        libssl-dev \
        libtool \
        libxml2-dev \
        libxslt-dev \
        libyaml-dev \
        make \
        patch \
        xz-utils \
        zlib1g-dev

WORKDIR /tmp/h2o/h2o
RUN git checkout $H2O_COMMITISH && \
        git submodule update --init --recursive && \
        cmake -DWITH_MRUBY=off . && \
        make && \
        make install
RUN useradd -ms /bin/bash h2o

WORKDIR /
RUN mkdir -p /usr/local/etc /usr/local/www-data /usr/local/var/log
COPY h2o.conf /usr/local/etc/
COPY ssl.pem /usr/local/etc/
COPY ssl.key /usr/local/etc/
RUN chmod 600 /usr/local/etc/ssl.*
ADD www-data /usr/local/www-data

RUN mkdir /run/h2o && chown h2o:h2o /run/h2o

EXPOSE 8080
EXPOSE 8443
CMD ["/usr/local/bin/h2o", "-c", "/usr/local/etc/h2o.conf"]
