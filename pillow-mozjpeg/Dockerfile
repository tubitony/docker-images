FROM ubuntu:xenial

RUN apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get -y --no-install-recommends install \
  # build tools
  nasm \
  build-essential \
  autoconf \
  automake \
  libtool \
  pkg-config \
  git \
  wget \
  libpng-dev \
  libtiff-dev \
  libjasper-dev \
  libdc1394-22-dev \
  libwebp-dev \
  ca-certificates \
  software-properties-common \
  && add-apt-repository ppa:deadsnakes/ppa \
  && apt-get update \
  && apt-get -y install python3.7 \
  python3.7-venv \
  python3.7-dev \
  # cleanup
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Download and compile mozjpeg
ENV MOZJPEG_VERSION="3.3.1"
RUN mkdir /mozjpeg-src \
  && wget https://github.com/mozilla/mozjpeg/archive/v${MOZJPEG_VERSION}.tar.gz -O /mozjpeg-src/v${MOZJPEG_VERSION}.tar.gz \
  && tar -xzf /mozjpeg-src/v${MOZJPEG_VERSION}.tar.gz -C /mozjpeg-src/
WORKDIR /mozjpeg-src/mozjpeg-${MOZJPEG_VERSION}
RUN autoreconf -fiv \
  && ./configure --with-jpeg8 \
  && make install prefix=/usr libdir=/usr/lib64 \
  && rm -r /mozjpeg-src
RUN echo "/usr/lib64\n" > /etc/ld.so.conf.d/mozjpeg.conf
RUN ldconfig

ENV VENV_PATH=/pyvenv

ENV PILLOW_VERSION="7.0.0"
RUN python3.7 -m venv ${VENV_PATH} \
  && . ${VENV_PATH}/bin/activate \
  && pip install --upgrade pip \
  && pip install pylint \
  # Only when installing from source, it can work with libjpeg-turbo(mozjpeg here)
  && pip install --no-binary=:all: pillow==${PILLOW_VERSION}

ENV PATH="${VENV_PATH}/bin:$PATH"

ENTRYPOINT ["python"]
