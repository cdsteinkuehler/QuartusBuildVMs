FROM cdsteinkuehler/jessie-quartus-base:latest
MAINTAINER Charles Steinkuehler <charles@steinkuehler.net>

USER root

# Web download
ADD http://download.altera.com/akdlm/software/acdsinst/15.1/185/ib_installers/QuartusLiteSetup-15.1.0.185-linux.run /tmp/
ADD http://download.altera.com/akdlm/software/acdsinst/15.1/185/ib_installers/cyclonev-15.1.0.185.qdz /tmp/
ADD http://download.altera.com/akdlm/software/acdsinst/15.1.2/193/update/QuartusSetup-15.1.2.193-linux.run /tmp/

# Local files
#ADD QuartusLiteSetup-15.1.0.185-linux.run /tmp/
#ADD cyclonev-15.1.0.185.qdz               /tmp/
#ADD QuartusSetup-15.1.2.193-linux.run     /tmp/

# Verify Quartus files
ADD sha1sums.txt /tmp/
RUN cd /tmp/ && sha1sum -c sha1sums.txt

# Fix file permissions
RUN cd /tmp/ && \
    chmod +x *.run && \
    chmod +r *

# Install Quartus
USER builder
RUN cd /tmp/ && \
    ./QuartusLiteSetup-15.1.0.185-linux.run --mode unattended

# Add Quartus bin directorys to the path
ENV PATH $PATH:/home/builder/altera_lite/15.1/quartus/bin:/home/builder/altera_lite/15.1/quartus/sopc_builder/bin

# Enable talkback (required to run SoC builds)
RUN xvfb-run -a tb2_install --enable

# Install sopc2dts
RUN cd && \
    git clone --depth 1 https://github.com/altera-opensource/sopc2dts && \
    make -C sopc2dts && \
    mkdir -p /usr/local/share/sopc2dts && \
    cp sopc2dts/sopc2dts.jar /usr/local/share/sopc2dts && \
    /bin/echo -e '#!/bin/bash -e\njava -jar /usr/local/share/sopc2dts/sopc2dts.jar $*\n' > /usr/local/bin/sopc2dts && \
    chmod +x /usr/local/bin/sopc2dts && \
    rm -rf sopc2dts

# Cleanup to save space
USER root
RUN rm -rf /tmp/* /home/builder/altera_lite/15.1/uninstall && \
    apt-get clean

USER builder

# !!!-WARNING-!!!
# Flatten the resulting image or it will be GIGANTIC!
#
# It is expected that at some point, Docker will have a native command
# for doing this, but for now the following method is required.
#
# Create a container from the image:
#   docker run <docker-image:big> /bin/true
#
# Find the container ID:
#   docker ps -a
#
# Get the docker-rebase script from here:
#   https://github.com/docbill/docker-scripts
#
# Rebase the big image to make it MUCH smaller:
#   docker-rebase <container-ID> <docker-image:little>
#
# Tag the image as needed:
#   docker tag <docker-image:little> <final-name:version>
#   docker tag <docker-image:little> <final-name:latest>
#
# You may want to limit your interface bandwidth to avoid killing your 'net connection:
#   sudo wondershaper eth0 500000 6000
#
# ...and push to the Docker hub:
#   docker push <final-name:version>
#   docker push <final-name:latest>
#
#   sudo wondershaper clear eth0
