# This Dockerfile is used to build an image containing basic stuff to be used as a Jenkins slave build node.
FROM bilalbaqar/jenkins-docker-slave:ubuntu-xenial
MAINTAINER Bilal Baqar <mbilalbaqar@gmail.com>

ENV DEBIAN_FRONTEND noninteractive

# install NODEJS, PIP, NPM and RUBYGEMS
RUN apt-get -q update &&\
    apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
      nodejs \
      python-pip \
      nodejs-legacy \
      npm \
      rubygems \
      hunspell &&\
    apt-get -q clean -y && rm -rf /var/lib/apt/lists/* && rm -f /var/cache/apt/*.bin


# install RVM, Ruby, and Bundler
RUN \gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
RUN \curl -L https://get.rvm.io | bash -s stable
RUN /bin/bash -c "source /etc/profile.d/rvm.sh"
RUN /bin/bash -l -c "rvm requirements"
RUN /bin/bash -l -c "rvm install 2.4.1"
RUN /bin/bash -l -c "rvm use 2.4.1 --default"
RUN /bin/bash -l -c "gem install bundler --no-ri --no-rdoc"

# install overcommit via gem
COPY files/Gemfile .
RUN /bin/bash -l -c "bundle install"

# install csslint, jshint, pydocstyble and pycodestyle
RUN /bin/bash -c "npm install -g csslint"
RUN /bin/bash -c "npm install -g jshint"
RUN /bin/bash -c "pip install pydocstyle"
RUN /bin/bash -c "pip install pycodestyle"

ENV DEBIAN_FRONTEND teletype

# Standard SSH port
EXPOSE 22

# Default command
CMD ["/usr/sbin/sshd", "-D"]
