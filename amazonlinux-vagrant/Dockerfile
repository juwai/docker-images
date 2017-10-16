FROM amazonlinux:2017.03

# Install required packages
RUN yum -y install yum-plugin-fastestmirror findutils \
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

RUN yum -y install sudo passwd unzip bzip2 cracklib-dicts python27-setuptools openssh-clients openssh-server \
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

# Install supervisord
RUN easy_install \
    'supervisor == 3.3.1' \
    'supervisor-stdout == 0.1.1'
ADD files/supervisord.conf /etc/supervisord.conf

# Configure sshd
ADD files/supervisor_sshd.conf /etc/supervisor.d/sshd.conf
RUN mkdir -p /var/run/sshd \
    && chmod 700 /var/run/sshd \
    && sed -i 's/GSSAPIAuthentication yes/GSSAPIAuthentication no/' /etc/ssh/sshd_config \
    && /usr/sbin/sshd-keygen

# Vagrant user
ADD files/insecure_key /tmp/insecure_key
RUN adduser vagrant \
    && usermod -aG wheel vagrant \
    && sudo -u vagrant mkdir -p /home/vagrant/.ssh \
    && chmod 700 /home/vagrant/.ssh \
    && sudo -u vagrant touch /home/vagrant/.ssh/authorized_keys \
    && chmod 600 /home/vagrant/.ssh/authorized_keys \
    && cat /tmp/insecure_key >> /home/vagrant/.ssh/authorized_keys \
    && echo 'vagrant' | passwd --stdin vagrant

# Sudo permission
RUN sed -i \
    -e 's~^# %wheel\tALL=(ALL)\tNOPASSWD: ALL~%wheel\tALL=(ALL) NOPASSWD: ALL~g' \
    -e 's~\(.*\) requiretty$~#\1requiretty~' \
    /etc/sudoers

# Add missing file
RUN echo 'NETWORKING=yes' > /etc/sysconfig/network

EXPOSE 22

CMD ["/usr/local/bin/supervisord", "--configuration=/etc/supervisord.conf"]
