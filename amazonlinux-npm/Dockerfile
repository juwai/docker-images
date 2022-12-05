FROM amazonlinux:2 as intermediate
ARG VERSION

# Install package tools
RUN yum -y install tar xz

# Download binary files
RUN cd tmp/ \
    && curl -o node.tar.xz https://nodejs.org/download/release/v$VERSION/node-v$VERSION-linux-x64.tar.xz \
    && unxz node.tar.xz \
    && tar xvf node.tar \
    && mv node-v$VERSION-linux-x64 node


FROM amazonlinux:2

# Install required packages
RUN yum -y install yum-plugin-fastestmirror \
    && yum -y install \
        git \
        gcc-c++ \
    && yum clean all \
    && find /usr/share \
        -type f \
        -regextype posix-extended \
        -regex '.*\.(jpg|png)$' \
        -delete \
    && rm -rf /etc/ld.so.cache \
    && rm -rf /sbin/sln \
    && rm -rf /usr/{{lib,share}/locale,share/{man,doc,info,cracklib,i18n},{lib,lib64}/gconv,bin/localedef,sbin/build-locale-archive} \
    && rm -rf /{root,tmp,var/cache/{ldconfig,yum}}/*

# Copy binary files from intermediate
COPY --from=intermediate /tmp/node /usr/local/node

# Create symlinks
RUN ln -s /usr/local/node/bin/node /usr/local/bin/node \
    && ln -s /usr/local/node/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm

# Disable host key checking for github.com
ADD files/ssh-config /root/.ssh/config
