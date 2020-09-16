FROM centos:7 as builder

# build and install Protobuf (dependency for CuraEngine)
RUN yum -y install curl git autoconf automake libtool curl make gcc-c++ unzip
WORKDIR /usr/local/
RUN git clone https://github.com/google/protobuf.git
WORKDIR /usr/local/protobuf/
RUN git submodule update --init --recursive
RUN ./autogen.sh
RUN ./configure && make && make install && ldconfig

# build and install libArcus (dependency for CuraEngine)
WORKDIR /usr/local/
RUN yum -y install cmake 
RUN git clone https://github.com/Ultimaker/libArcus.git
RUN cd libArcus && git checkout 41b8ed7ad40c99a543e39ad696c890b05655978e
RUN mkdir /usr/local/libArcus/build/ 
WORKDIR /usr/local/libArcus/build/
RUN cmake .. -DBUILD_PYTHON=OFF && make && make install


# build and install CuraEngine
WORKDIR /usr/local/
RUN git clone https://github.com/Ultimaker/CuraEngine --branch 3.4 --depth 1
RUN mkdir /usr/local/CuraEngine/build/
RUN rm /usr/local/CuraEngine/CMakeLists.txt
COPY ./CMakeLists.txt /usr/local/CuraEngine
#install a good version of gcc
RUN yum -y install centos-release-scl
RUN yum-config-manager --enable rhel-server-rhscl-7-rpms
RUN yum -y install devtoolset-7
WORKDIR /usr/local/CuraEngine/build/
RUN source scl_source enable devtoolset-7 && cmake .. && make

FROM centos:7

RUN yum -y install make

# Copy Protobuf
COPY --from=builder /usr/local/lib/libprotobuf.so /usr/local/lib/libprotobuf.so.24
# Copy libArcus
COPY --from=builder /usr/local/libArcus/build/libArcus.so.1.1.0 /usr/local/lib/libArcus.so.3

COPY --from=builder /usr/lib64/libgomp.so.1.0.0 /usr/lib64/libgomp.so.1

COPY --from=builder /usr/local/CuraEngine/build/CuraEngine /usr/local/bin/CuraEngine

ARG UNAME=cura
ARG UID=1000
ARG GID=1000
RUN groupadd -g $GID $UNAME
RUN adduser -u $UID $UNAME -g $UNAME

USER $UNAME

ENTRYPOINT [ "CuraEngine" ]