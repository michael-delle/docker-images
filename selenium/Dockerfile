FROM selenium/standalone-chrome
LABEL maintainer="michael@michaeldelle.com"

USER root

RUN apt-get update -qq \
        && apt-get install -yqq \
            python3-distutils \
            python3-pip; \
    pip3 install pytest selenium requests;