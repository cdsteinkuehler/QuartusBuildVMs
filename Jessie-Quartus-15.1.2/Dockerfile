FROM cdsteinkuehler/jessie-quartus-base:latest
MAINTAINER Charles Steinkuehler <charles@steinkuehler.net>

# Web download
#ADD http://download.altera.com/akdlm/software/acdsinst/15.1/185/ib_installers/QuartusLiteSetup-15.1.0.185-linux.run /tmp/
#ADD http://download.altera.com/akdlm/software/acdsinst/15.1/185/ib_installers/cyclonev-15.1.0.185.qdz /tmp/
#ADD http://download.altera.com/akdlm/software/acdsinst/15.1.2/193/update/QuartusSetup-15.1.2.193-linux.run /tmp/

# Local files
ADD QuartusLiteSetup-15.1.0.185-linux.run /tmp/
ADD cyclonev-15.1.0.185.qdz               /tmp/
ADD QuartusSetup-15.1.2.193-linux.run     /tmp/

# Verify Quartus files
ADD sha1sums.txt /tmp/


# create builder user
# Install sopc2dts as root
RUN groupadd -r builder -g 815 && \
    useradd -u 813 -r -g builder -m -d /home/builder -s /bin/bash builder && \
    git clone --depth 1 https://github.com/altera-opensource/sopc2dts && \
    make -C sopc2dts && \
    mkdir -p /usr/local/share/sopc2dts && \
    cp sopc2dts/sopc2dts.jar /usr/local/share/sopc2dts && \
    /bin/echo -e '#!/bin/bash -e\njava -jar /usr/local/share/sopc2dts/sopc2dts.jar $*\n' > /usr/local/bin/sopc2dts && \
    chmod +x /usr/local/bin/sopc2dts && \
    rm -rf sopc2dts && \
    cd /tmp/ && sha1sum -c sha1sums.txt && \
    chmod +x *.run

# Install Quartus as user builder
# Enable talkback (required to run SoC builds)
USER builder

ENV HOME /home/builder

# Add Quartus bin directorys to the path
ENV PATH "$PATH:/home/builder/altera_lite/15.1/quartus/bin/:/home/builder/altera_lite/15.1/quartus/sopc_builder/bin/"

RUN cd /tmp/ && \
    ./QuartusLiteSetup-15.1.0.185-linux.run --mode unattended &&  \
    xvfb-run -a tb2_install --enable

# relax permissions on /home/builder so non-builder users can execute quartus
# Cleanup to save space
USER root

RUN rm -rf /tmp/* /home/builder/altera_lite/15.1/uninstall && \
    rm -rf /var/lib/apt/lists/* && \
    chmod -R go+xr /home/builder && mv /home/builder /home/builder_new && mv /home/builder_new /home/builder
#                           ^---------------------^
# workaround weird bug: https://github.com/docker/docker/issues/783#issuecomment-212907954

