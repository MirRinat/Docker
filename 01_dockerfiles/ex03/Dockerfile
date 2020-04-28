FROM ubuntu:16.04

RUN apt-get update && apt-get upgrade -y &&\
    apt-get install -y \
    curl \
    openssh-server \
    ca-certificates \
    postfix
WORKDIR /
RUN curl -LO  https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh

RUN bash script.deb.sh
RUN apt-get update && EXTERNAL_URL="http://gitlab.example.com" apt-get install -yq gitlab-ce

RUN apt update && apt install -y tzdata && \
  apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 443 80 22

ENTRYPOINT (/opt/gitlab/embedded/bin/runsvdir-start&) && gitlab-ctl reconfigure && tail -f /dev/null
