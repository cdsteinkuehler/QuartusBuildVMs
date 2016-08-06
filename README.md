# QuartusBuildVMs

Details for installing Quartus on Linux (via VMs and Docker containers) to create build machines for Altera FPGA projects

## Jessie-Quartus-Base

This is a minimal Debian Jessie install, based on debian:jessie that
adds the Debian packages required to run Quartus and the Machinekit
build process (including some things like ProtoBuf tools for formatting
the Firmware-ID message that gets added to the FPGA logic as a ROM).

Build this normally using the Dockerfile and push to the Docker hub.

## Jessie-Quartus-15.1.2

This Dockerfile installs the Quartus 15.1.2 toolchain.  Review and
adjust the Dockerfile if you have a local copy of the Quartus install
files already downloaded.  After you build the docker image using the
Dockerfile, it will be **HUGE**, and unsutiable for sharing on the
Docker hub.  To reduce the size, the image is flattened using the
docker-rebase tool from docbill (details are in the Dockerfile).  The
flattened file should be shared on Dockerhub.

If you are only trying to build FPGA bitfiles locally, you may not want
or need to flatten the docker image.  It just depends on how much disk
space you are willing to consume.

