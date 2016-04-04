FROM cdsteinkuehler/jessie-quartus-base:latest
MAINTAINER Charles Steinkuehler <charles@steinkuehler.net>

# Web download
#ADD http://download.altera.com/akdlm/software/acdsinst/15.1/185/ib_installers/QuartusLiteSetup-15.1.0.185-linux.run \
#    /tmp/
#ADD http://download.altera.com/akdlm/software/acdsinst/15.1/185/ib_installers/cyclonev-15.1.0.185.qdz \
#    /tmp/
#ADD http://download.altera.com/akdlm/software/acdsinst/15.1.2/193/update/QuartusSetup-15.1.2.193-linux.run \
#    /tmp/
# Check downloaded file checksums!

# Local files
ADD QuartusLiteSetup-15.1.0.185-linux.run /tmp/
ADD cyclonev-15.1.0.185.qdz               /tmp/
ADD QuartusSetup-15.1.2.193-linux.run     /tmp/

RUN cd /tmp/ && \
    chmod +x *.run && \
    ./QuartusLiteSetup-15.1.0.185-linux.run --mode unattended

ENV PATH $PATH:/root/altera_lite/15.1/quartus/bin:/root/altera_lite/15.1/quartus/sopc_builder/bin
RUN xvfb-run -a tb2_install --enable

