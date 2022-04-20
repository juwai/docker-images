FROM amazonlinux:2 as intermediate

# Install dependencies, nginx, and useful tools
RUN yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm \
    && yum -y install nginx shadow-utils procps nc net-tools iputils telnet which tree unzip


FROM amazonlinux:2

# Copy files from intermediate image
COPY --from=intermediate /etc/group /etc/group
COPY --from=intermediate /etc/nginx /etc/nginx
COPY --from=intermediate /etc/passwd /etc/passwd
COPY --from=intermediate /usr/bin /usr/bin
COPY --from=intermediate /usr/lib64 /usr/lib64
COPY --from=intermediate /usr/libexec /usr/libexec
COPY --from=intermediate /usr/sbin /usr/sbin
COPY --from=intermediate /usr/share/nginx /usr/share/nginx
COPY --from=intermediate /var/lib/nginx /var/lib/nginx

# Copy nginx config file
COPY nginx.conf /etc/nginx/conf.d/php-app.conf

# Create log directory
RUN mkdir -p /var/log/nginx/php-app

CMD ["/usr/sbin/nginx", "-g", "daemon off;"]
