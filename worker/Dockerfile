# please follow docker best practices
# https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/

# Provides a base CentOS image with latest buildbot worker installed

FROM        centos:centos7
MAINTAINER  Jacob Kozol

# Install security updates and required packages
RUN \
    yum -y update && \
    yum -y install epel-release && \
    yum -y groups install "Development Tools" &&\
    yum -y install \
    	git \
    	subversion \
    	python-devel \
	python-pip \
    	libffi-devel \
    	openssl-devel \
    	curl && \ 
    # so we need to simulate that here.  See https://github.com/Yelp/dumb-init
    # curl https://github.com/Yelp/dumb-init/releases/download/v1.2.1/dumb-init_1.2.1_amd64.deb -Lo /tmp/init.deb && dpkg -i /tmp/init.deb &&\
    # Install required python packages, and twisted
    pip install --upgrade pip setuptools wheel && \
    pip install dumb-init && \
    pip --no-cache-dir install 'twisted[tls]' && \
    mkdir /buildbot &&\
    useradd -ms /bin/bash buildbot && chown -R buildbot /buildbot

COPY . /usr/src/buildbot-worker
COPY docker/buildbot.tac /buildbot/buildbot.tac

RUN pip install /usr/src/buildbot-worker

USER buildbot
WORKDIR /buildbot

CMD ["/usr/bin/dumb-init", "twistd", "-ny", "buildbot.tac"]
