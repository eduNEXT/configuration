# docker build -f docker/build/ecommerce/Dockerfile . -t edxops/ecommerce
FROM edxops/xenial-common:latest
MAINTAINER edxops

ADD . /edx/app/edx_ansible/edx_ansible

WORKDIR /edx/app/edx_ansible/edx_ansible/docker/plays

RUN echo '{ "allow_root": true }' > /root/.bowerrc

RUN apt-get update
RUN apt install -y xvfb firefox gettext

ARG ECOMMERCE_VERSION=master
ENV ECOMMERCE_VERSION=${ECOMMERCE_VERSION}

ARG COMMON_GIT_PATH="eduNEXT"
ENV COMMON_GIT_PATH=${COMMON_GIT_PATH}

COPY docker/build/ecommerce/ansible_overrides.yml /
RUN /edx/app/edx_ansible/venvs/edx_ansible/bin/ansible-playbook ecommerce.yml \
	-i '127.0.0.1,' \
	-c local \
	-t "install:base,install:system-requirements,install:configuration,install:app-requirements,install:code" \
	--extra-vars=ECOMMERCE_VERSION=${ECOMMERCE_VERSION} \
    --extra-vars=COMMON_GIT_PATH=${COMMON_GIT_PATH} \
	-e@/ansible_overrides.yml
COPY docker/build/ecommerce/docker-run.sh /

CMD ["/docker-run.sh"]
EXPOSE 8130
