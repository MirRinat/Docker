FROM debian:stable

ENV DEBIAN_FRONTEND=noninteractive
ARG GITLAB_IP=localhost


RUN apt-get update -y && \
	apt-get upgrade -y && \
	apt-get install -y curl openssh-server ca-certificates git tzdata

RUN curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash

RUN apt-get update -y && apt-get install -y gitlab-ce

# Create self-signed certificate
RUN mkdir -p /etc/gitlab/ssl && \
	chmod 700 /etc/gitlab/ssl && \
	openssl req -new -x509 -nodes -newkey rsa:2048 -keyout /etc/gitlab/ssl/gitlab.key -out /etc/gitlab/ssl/gitlab.crt -days 365 -subj "/C=RU/ST=Moscow/L=Moscow/O=21-school/OU=docker-1/CN=${GITLAB_IP}" && \
	sed -i -e "s|^\(external_url\) .*$|\1 'https://${GITLAB_IP}'|" \
	-e "s|^# \(nginx\['ssl_certificate'\] = \"/etc/gitlab/ssl\)/.*$|\1/gitlab.crt\"|" \
	-e "s|^# \(nginx\['ssl_certificate_key'\] = \"/etc/gitlab/ssl\)/.*$|\1/gitlab.key\"|" \
	-e "s|^# \(gitlab_rails\['gitlab_shell_ssh_port'\] = 22\)$|\1|" \
	/etc/gitlab/gitlab.rb

EXPOSE 443
EXPOSE 22

ENTRYPOINT  service ssh start && \
				(/opt/gitlab/embedded/bin/runsvdir-start &) && \
				gitlab-ctl reconfigure && \
				tail -f /dev/null


# docker build --build-arg GITLAB_IP=$(docker-machine ip Char) -t ex03 .
# docker run --rm -it -p 8000:443 -p 8022:22 --privileged ex03
# https://192.168.99.X:8000
#HTTPS
# env GIT_SSL_NO_VERIFY=true git clone https://$(docker-machine ip Char):8000/root/test.git

# env GIT_SSL_NO_VERIFY=true git push

# SSH
# ssh -T -p 8022 git@$(docker-machine ip Char)
# env GIT_SSH_COMMAND='ssh -i id_ed25519 -p 8022' git clone git@$(docker-machine ip Char):root/test.git

# env GIT_SSH_COMMAND='ssh -p 8022' git push
