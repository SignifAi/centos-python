FROM centos:centos7

# http://bugs.python.org/issue19846
# > At the moment, setting "LANG=C" on a Linux system *fundamentally breaks Python 3*, and that's not OK.
ENV LANG C.UTF-8
ENV LC_ALL en_US.UTF-8

ENV PYTHON_VERSION 3.7.3

# if this is called "PIP_VERSION", pip explodes with "ValueError: invalid truth value '<VERSION>'"
ENV PYTHON_PIP_VERSION 18.0

ENV PYCURL_SSL_LIBRARY nss

RUN set -ex \
    && yum -y update \
    && yum -y groupinstall 'Development Tools' \
    && yum -y install \
    kernel-devel \
    ImageMagick-devel \
    bzip2-devel \
    libcurl \
    libcurl-devel \
    openssl \
    openssl-devel \
    libevent-devel \
    libffi-devel \
    glib2-devel \
    libjpeg-devel \
    mysql-devel \
    postgresql-devel \
    ncurses-devel \
    readline \
    sqlite \
    readline-devel \
    sqlite-devel \
    libxml2-devel \
    libxslt-devel \
    zlib-devel \
    libyaml-devel \
    wget \
    bzr \
    git \
    tar \
    bind-utils \
    which \
    && yum clean all \
    && rm -rf /var/cache/yum \
    && curl -SL "https://www.python.org/static/files/pubkeys.txt" -o pubkeys.txt \
    && gpg --import pubkeys.txt \
    && mkdir -p /usr/src/python \
    && curl -SL "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz" -o python.tar.xz \
    && curl -SL "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz.asc" -o python.tar.xz.asc \
    && gpg --verify python.tar.xz.asc \
    && tar -xJC /usr/src/python --strip-components=1 -f python.tar.xz \
    && rm -f python.tar.xz* \
    && cd /usr/src/python \
    && ./configure --enable-shared --enable-optimizations --with-lto \
    && make -j$(nproc) \
    && make -j$(nproc) altinstall \
    && echo "/usr/local/lib/" > /etc/ld.so.conf.d/local.conf \
    && ldconfig \
    && pip3.7 install --no-cache-dir --upgrade --ignore-installed pip==$PYTHON_PIP_VERSION \
    && find /usr/local \
        \( -type d -a -name test -o -name tests \) \
        -o \( -type f -a -name '*.pyc' -o -name '*.pyo' \) \
        -exec rm -rf '{}' + \
    && rm -rf /usr/src/python \
    && cd /usr/local/bin \
    && ln -s easy_install-3.7 easy_install3 \
    && ln -s idle3.7 idle3 \
    && ln -s pydoc3.7 pydoc3 \
    && ln -s python3.7 python3 \
    && ln -s python-config3.7 python-config3 \
    && pip3.7 install --no-cache-dir --upgrade pipenv
