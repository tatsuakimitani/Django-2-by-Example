# "ported" by Adam Miller <maxamillion@fedoraproject.org> from
#   https://github.com/fedora-cloud/Fedora-Dockerfiles
#
# Originally written for Fedora-Dockerfiles by
#   scollier <scollier@redhat.com>

FROM centos:centos7
MAINTAINER The CentOS Project <cloud-ops@centos.org>

RUN yum -y update; yum clean all
RUN yum -y install sudo epel-release; yum clean all
RUN yum -y install postgresql-server postgresql postgresql-contrib supervisor pwgen; yum clean all

# Install Python and Node.js
RUN yum install -y https://centos7.iuscommunity.org/ius-release.rpm epel-release
RUN yum install -y wget unzip iproute python36u python36u-libs python36u-devel python36u-pip sudo

RUN yum -y remove git
#RUN yum -y install https://centos7.iuscommunity.org/ius-release.rpm
RUN yum -y install git2u yum-utils


# Install tools
ADD requirements.txt .
RUN pip3.6 install flake8 nose coverage
RUN pip3.6 install awscli --upgrade --user
RUN pip3.6 install -r requirements.txt

RUN sed -i -e 's/^#\s%wheel\s*ALL=(ALL)\s*ALL$/%wheel\tALL=(ALL)\tALL/g' /etc/sudoers
RUN sed -i -e 's/^#\s%wheel\s*ALL=(ALL)\s*NOPASSWD: ALL$/jenkins\tALL=(ALL)\tNOPASSWD: ALL/g' /etc/sudoers

RUN groupadd -g 991 jenkins
RUN useradd -u 995 -g 991 jenkins
RUN usermod -aG wheel jenkins


ADD ./postgresql-setup /usr/bin/postgresql-setup
ADD ./supervisord.conf /etc/supervisord.conf
ADD ./start_postgres.sh /start_postgres.sh

#Sudo requires a tty. fix that.
RUN sed -i 's/.*requiretty$/#Defaults requiretty/' /etc/sudoers
RUN chmod +x /usr/bin/postgresql-setup
RUN chmod +x /start_postgres.sh

RUN /usr/bin/postgresql-setup initdb

ADD ./postgresql.conf /var/lib/pgsql/data/postgresql.conf

RUN chown -v postgres.postgres /var/lib/pgsql/data/postgresql.conf

RUN sed -i -e 's|local   all|#local   all|g' /var/lib/pgsql/data/pg_hba.conf
RUN sed -i -e 's|host    all|#host    all|g' /var/lib/pgsql/data/pg_hba.conf

RUN echo "host    all             all             0.0.0.0/0               trust" >> /var/lib/pgsql/data/pg_hba.conf

VOLUME ["/var/lib/pgsql"]

EXPOSE 5432

CMD ["/bin/bash", "/start_postgres.sh"]
