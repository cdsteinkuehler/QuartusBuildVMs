FROM cdsteinkuehler/jessie-quartus-15.1.2
MAINTAINER Michael Haberler <haberlerm@gmail.com>

RUN apt-get update && \
    apt-get -y install \
	openjdk-7-jdk && \
    git clone --depth 1 https://github.com/altera-opensource/sopc2dts && \
    make -C sopc2dts && \
    mkdir -p /usr/local/share/sopc2dts && \
    cp sopc2dts/sopc2dts.jar /usr/local/share/sopc2dts && \
    /bin/echo -e '#!/bin/bash -e\njava -jar /usr/local/share/sopc2dts/sopc2dts.jar $*\n' > /usr/local/bin/sopc2dts && \
    chmod +x /usr/local/bin/sopc2dts && \
    rm -rf sopc2dts


# Add Quartus bin directorys to the path
ENV PATH $PATH:/root/altera_lite/15.1/quartus/bin:/root/altera_lite/15.1/quartus/sopc_builder/bin
