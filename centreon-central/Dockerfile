FROM centos:latest

# Set up base packages that are expected
RUN dnf -y install openssh-server crontabs NetworkManager firewalld selinux-policy

RUN systemctl mask dev-mqueue.mount dev-hugepages.mount \
     systemd-remount-fs.service sys-kernel-config.mount \
     sys-kernel-debug.mount sys-fs-fuse-connections.mount \
     graphical.target systemd-logind.service \
     NetworkManager.service systemd-hostnamed.service

STOPSIGNAL SIGRTMIN+3

# Centreon préconfig
RUN dnf -y update
RUN sed -i s/^SELINUX=.*$/SELINUX=disabled/ /etc/selinux/config

#RUN systemctl stop firewalld
RUN systemctl disable firewalld
RUN dnf -y install dnf-plugins-core epel-release
RUN dnf config-manager --set-enabled powertools

# Centreon install
RUN dnf install -y http://yum.centreon.com/standard/20.10/el8/stable/noarch/RPMS/centreon-release-20.10-2.el8.noarch.rpm
RUN dnf install -y centreon centreon-database

# Centreon configuration
RUN echo "date.timezone = Europe/Paris" >> /etc/php.d/50-centreon.ini
RUN systemctl enable php-fpm httpd mariadb centreon cbd centengine gorgoned snmptrapd centreontrapd snmpd

STOPSIGNAL SIGRTMIN+3

EXPOSE 80

CMD ["/sbin/init"]
