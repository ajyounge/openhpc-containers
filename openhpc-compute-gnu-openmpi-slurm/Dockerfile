# Author: Andrew J. Younge ajyoung@sandia.gov
# Content: Dockerfile for creating OpenHPC compute images with GNU compiler, 
# OpenMPI, SLURM client, and various other libraries, including Trilinos.
# Inspiration taken from koiz image


FROM centos:7

#Variables
ENV OPENHPC_VERSION     1.3

# Install basic non OpenHPC software
RUN yum -y install openssh-server openssh-clients

# Enable OpenHPC repository for local use
RUN yum -y install http://build.openhpc.community/OpenHPC:/${OPENHPC_VERSION}/CentOS_7/x86_64/ohpc-release-${OPENHPC_VERSION}-1.el7.x86_64.rpm epel-release

# Compute package
RUN yum -y groupinstall ohpc-base-compute

# Add Slurm client support && Include modules user environment
RUN yum -y groupinstall ohpc-slurm-client && \
    yum -y install lmod-ohpc 

# Install OpenMPI with PSM2 to have optional OPA support if needed
RUN yum -y install openmpi-psm2-gnu-ohpc

# Setup default development environment
RUN yum -y install lmod-defaults-gnu-openmpi-ohpc

# Install 3rd part libaries and support services
RUN yum -y groupinstall ohpc-parallel-libs-gnu-openmpi && \
	yum -y install trilinos-gnu-openmpi-ohpc



RUN mkdir /var/run/sshd
RUN echo 'root:screencast' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile



EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]

