# nginx-1.19
#FROM nginx:stable-alpine
FROM nginx:1.19.5-alpine
LABEL maintainer="Tomasz Chmiel <tchmiel@icis.pcz.pl>"

LABEL io.k8s.description="Nginx" \
      io.k8s.display-name="Nginx builder 1.0" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="nginx,openshift"

EXPOSE 8080

# Work as root
# User is overwritten by Openshift afterwards
USER root

# Install useful tools
RUN apk add --no-cache curl bash openssl ca-certificates

# Remove obsolete files
RUN rm -f /etc/nginx/conf.d/default.conf

# Creating resolvers.conf
#/etc/nginx/resolvers.conf
RUN echo resolver $(awk 'BEGIN{ORS=" "} $1=="nameserver" {print $2}' /etc/resolv.conf)"ipv6=off;" > /etc/nginx/resolvers.conf

RUN mkdir -p /var/run/nginx && mkdir -p /var/log/nginx

# Set permissions
RUN chmod g+rwx /var/cache/nginx /var/run /var/log/nginx && \
    chgrp -R root /var/cache/nginx && \
    chmod -R go+w /usr/share/nginx/


LABEL io.openshift.s2i.scripts-url=image:///usr/local/s2i
COPY ./s2i/bin/ /usr/local/s2i

#RUN chown -R 1001:1001 /opt/openshift
#RUN chown -R 1001:1001 /opt/app-root

# TODO: Drop the root user and make the content of /opt/app-root owned by user 1001
# RUN chown -R 1001:1001 /opt/app-root

WORKDIR /usr/share/nginx/html

CMD ["usage"]