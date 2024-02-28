FROM gcc:7.3.0

RUN echo "deb http://archive.debian.org/debian/ stretch main" > /etc/apt/sources.list && \
    echo "deb-src http://archive.debian.org/debian/ stretch main" >> /etc/apt/sources.list

RUN apt-get -qq update
RUN apt-get -qq upgrade

# Install necessary packages to build cmake
RUN apt-get -qq install wget libssl-dev

# Download, build and install CMake
WORKDIR /opt
RUN wget https://github.com/Kitware/CMake/releases/download/v3.15.0/cmake-3.15.0.tar.gz && \
    tar -xzvf cmake-3.15.0.tar.gz && \
    cd cmake-3.15.0 && \
    ./bootstrap && \
    make && \
    make install

RUN apt-get -qq install libboost-all-dev=1.62.0.1
RUN apt-get -qq install build-essential libtcmalloc-minimal4 && \
  ln -s /usr/lib/libtcmalloc_minimal.so.4 /usr/lib/libtcmalloc_minimal.so

WORKDIR /usr/src

RUN git clone https://github.com/mongodb/mongo-c-driver.git \
&& cd mongo-c-driver && git checkout 1.10.1 \
&& mkdir cmake-build && cd cmake-build \
&& cmake -DENABLE_AUTOMATIC_INIT_AND_CLEANUP=OFF .. \
&& make && make install && ldconfig /usr/local/lib

# Clone and install mnmlstc/core
WORKDIR /opt
RUN git clone https://github.com/mnmlstc/core.git \
    && cd core \
    && mkdir build \
    && cd build \
    && cmake .. \
    && make && make install

RUN git clone https://github.com/mongodb/mongo-cxx-driver.git \
--branch releases/stable --depth 1 \
&& cd mongo-cxx-driver/build && cmake \
-DBSONCXX_POLY_USE_MNMLSTC=1 \
-DCMAKE_BUILD_TYPE=Release \
-DCMAKE_INSTALL_PREFIX=/usr/local \
-DLIBMONGOC_DIR=/usr/lib/x86_64-linux-gnu \
-DLIBBSON_DIR=/usr/lib/x86_64-linux-gnu \
-DCMAKE_MODULE_PATH=/usr/src/mongo-cxx-driver-r3.0.3/cmake .. \
&& make && make install && ldconfig /usr/local/lib
