# Based off of the official docker buildbot-master image

# Use a multi-stage build: 
# https://docs.docker.com/develop/develop-images/multistage-build/ 
# 
# Provide an intermediate Docker image named "buildbot-build". 
# This intermediate image builds binary wheels # which get installed in the final image. 
# This allows us to avoid installing build tools like gcc in the final image.

FROM centos:centos7 AS buildbot-build
MAINTAINER "Jacob Kozol" <jkozol@redhat.com>

COPY . /usr/src/buildbot/

RUN yum -y update; yum clean all
RUN yum -y install epel-release
RUN \
    yum -y install \
	git \
	gcc \
	libcurl-devel \
	python-devel \
	libfi-devel \ 
	openssl-devel \
	postgresql-devel \
	python-pip &&  \
    pip install --upgrade pip setuptools wheel && \
    pip install "buildbot[bundle,tls]" && \
    pip install -r /usr/src/buildbot/requirements-docker-extras.txt && \
    pip install dumb-init 

WORKDIR /var/lib/buildbot
CMD ["dumb-init", "/usr/src/buildbot/docker/start_buildbot.sh"]
