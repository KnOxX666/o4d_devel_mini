#FROM registry.redhat.io/rhel9/gcc-toolset-13-toolchain:latest
#FROM registry.redhat.io/rhel8/gcc-toolset-13-toolchain:latest
#FROM quay.io/centos/centos:stream9
FROM rockylinux:8.9

USER root

# fixuid, add container to groups docker and all (at Orthogon docker is granted for users in group 'all')
# use fixuid in developer containers only for security reasons
RUN groupadd -g 1000 container && \
    useradd -u 1000 -g container -d /home/container -s /bin/bash container && \
    groupadd -g 137 docker && usermod -aG docker container && \
    groupadd -g 7052 all && usermod -aG all container
RUN USER=container && \
    GROUP=container && \
    curl -SsL https://github.com/boxboat/fixuid/releases/download/v0.6.0/fixuid-0.6.0-linux-amd64.tar.gz | tar -C /usr/local/bin -xzf - && \
    chown root:root /usr/local/bin/fixuid && \
    chmod 4755 /usr/local/bin/fixuid && \
    mkdir -p /etc/fixuid && \
    printf "user: $USER\ngroup: $GROUP\n" > /etc/fixuid/config.yml


# Needed for tar to be able to extract *.tar.bz2 below:
RUN /bin/yum install --assumeyes bzip2

# Additional development packages (compile time)
RUN /bin/yum install --assumeyes gcc-c++ 
RUN /bin/yum install --assumeyes libtirpc-devel

# OPScenter runtime
#RUN /bin/yum install --assumeyes openssl

# Packages during development for convenience (some already installed by default)
#RUN /bin/yum install --assumeyes bash zsh screen less vim nano openssh-clients iputils wget gnuplot
RUN /bin/yum install --assumeyes bash less vim

# Cleanup
RUN yum clean all ; rm -Rf /var/cache/yum/*

# Aliases
RUN echo 'alias ll="ls -l"' >> /etc/bashrc

# This is a company hack to avoid prefix ./ in case executable is run in current dir 
# (lot of scripts prepared in company needs this configuration:
#ENV PATH=${PATH}:.

ENV OS=rheldev
ENV OS_MAJOR=8
ENV OS_MINOR=9
ENV MACH=x86_64

USER container
ENTRYPOINT ["fixuid", "-q"]
