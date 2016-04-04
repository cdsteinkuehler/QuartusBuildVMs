FROM cdsteinkuehler/jessie-quartus-base:latest
MAINTAINER Charles Steinkuehler <charles@steinkuehler.net>

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

# Install Quartus
RUN cd /tmp/ && \
    chmod +x *.run && \
    ./QuartusLiteSetup-15.1.0.185-linux.run --mode unattended

# Add Quartus bin directorys to the path
ENV PATH $PATH:/root/altera_lite/15.1/quartus/bin:/root/altera_lite/15.1/quartus/sopc_builder/bin

# Enable talkback (required to run SoC builds)
RUN xvfb-run -a tb2_install --enable

# Cleanup to save space
RUN rm -rf /tmp/*

