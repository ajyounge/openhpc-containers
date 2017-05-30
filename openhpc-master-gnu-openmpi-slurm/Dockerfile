# Author: Andrew J. Younge ajyoung@sandia.gov
# Content: Dockerfile for creating OpenHPC master head node image with GNU
# compiler, OpenMPI, SLURM client, and various other libraries, including 
# Trilinos.
# 
# Inspiration taken from koiz image


FROM centos:7

#Variables
ENV OPENHPC_VERSION     1.3

# Install basic non OpenHPC software
RUN yum -y install openssh-server openssh-clients

# Enable OpenHPC repository for local use
RUN yum -y install http://build.openhpc.community/OpenHPC:/${OPENHPC_VERSION}/CentOS_7/x86_64/ohpc-release-${OPENHPC_VERSION}-1.el7.x86_64.rpm epel-release


#Install Master base and SLURM server
RUN yum -y groupinstall ohpc-base && \
	yum -y groupinstall ohpc-slurm-server

#Setup NTP
RUN systemctl enable ntpd.service && \
	echo "server wwv.nist.gov" >> /etc/ntp.conf

    

# Install OpenMPI with PSM2 to have optional OPA support if needed
RUN yum -y install openmpi-psm2-gnu-ohpc && \
	yum -y install lmod-ohpc 

# Setup default development environment
RUN yum -y install lmod-defaults-gnu-openmpi-ohpc

# Install 3rd part libaries and support services
RUN yum -y groupinstall ohpc-parallel-libs-gnu-openmpi && \
	yum -y install trilinos-gnu-openmpi-ohpc



EXPOSE 22

ENTRYPOINT /bin/bash
