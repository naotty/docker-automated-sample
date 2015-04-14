FROM centos:centos6
MAINTAINER tnaototo

USER root
WORKDIR /root

# yum
RUN yum install -y http://linux.mirrors.es.net/fedora-epel/6Server/x86_64/epel-release-6-8.noarch.rpm
RUN yum update -y
RUN yum --nogpgcheck install -y passwd sudo gcc gcc-c++ make git openssl-devel zlib-devel bzip2 bzip2-devel libevent-devel openssh openssh-server readline-devel tar expat-devel ntp mysql-devel hostname python-pip
RUN yum install -y http://nginx.org/packages/centos/6/noarch/RPMS/nginx-release-centos-6-0.el6.ngx.noarch.rpm
RUN yum install -y --enablerepo=nginx nginx
RUN yum clean all

# supervisord
RUN /usr/bin/pip install supervisor

# ruby
RUN git clone https://github.com/tagomoris/xbuild
RUN xbuild/ruby-install 2.1.2 /opt/ruby

#CMD /bin/bash

# nginx
RUN mv /etc/nginx/nginx.conf ~
ADD nginx.conf /etc/nginx/

# supervisord
ADD supervisord.conf /etc/

# application
WORKDIR /app
ENV PATH /opt/ruby/bin:$PATH
ADD SampleApp /app
RUN bundle install --deployment
ENV SECRET_KEY_BASE 75c7eb959bc7994feb7eb17bc76079cba5fc85121fd17a07ca57117450534a3ceda1729e8dca937a3d7f554f8e9f33ac16b98de3d2081938dddad7b4e49f4634

EXPOSE 80
CMD ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisord.conf"]
