# Use phusion/baseimage as base image. To make your builds reproducible, make
# sure you lock down to a specific version, not to `latest`!
# See https://github.com/phusion/baseimage-docker/blob/master/Changelog.md for
# a list of version numbers.
FROM phusion/baseimage:0.9.12
MAINTAINER Simón Castillo, <simoncastillo@gmail.com>

# Set correct environment variables.
ENV HOME /root

# Regenerate SSH host keys. baseimage-docker does not contain any, so you
# have to do that yourself. You may also comment out this instruction; the
# init system will auto-generate one during boot.
RUN /etc/my_init.d/00_regen_ssh_host_keys.sh

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

# WARNING: only for development environments!
RUN /usr/sbin/enable_insecure_key

# Fix locales
RUN locale-gen en_US.UTF-8
RUN dpkg-reconfigure locales
RUN echo "LC_ALL=en_US.UTF-8" > /etc/environment
RUN echo "LANG=en_US.UTF-8" > /etc/environment

# Install some basic utilities.
RUN apt-get update
RUN apt-get dist-upgrade -y -q
RUN apt-get install -y -q software-properties-common command-not-found
RUN apt-get install -y -q build-essential tig man atool openssh-server
RUN apt-get install -y -q zsh wget curl tmux htop aptitude psmisc
RUN apt-get install -y -q emacs24-nox nano vim python-pygments
RUN apt-get install -y -q git subversion atfs
RUN apt-get install -y -q --no-install-recommends mercurial

# Some interfacing with PostgreSQL
# - server dev to build drivers
# - client for psql
RUN apt-get install -y -q postgresql-server-dev-9.3 postgresql-client-9.3

# Install MySQL client
RUN apt-get install -y -q mysql-client

# Node.js stuff
# - coffee script
# - grunt
# - gulp
# - yeoman
# - bower
RUN add-apt-repository ppa:chris-lea/node.js -y
RUN apt-get -y -q update
RUN apt-get install -y -q nodejs
RUN npm install -g coffee-script bower grunt-cli gulp yo

# Python stuff
# - pip
# - setup tools
# - dev files
RUN apt-get install -y -q python python-pip python-dev python-setuptools
RUN pip install ipython

# Ruby stuff
# - compass
# - bundler
# - rake
RUN apt-get install -y -q ruby ruby1.9.1-dev
RUN gem install compass bundler rake

# # Go
# RUN apt-get install -y golang

# Change to ZSH
RUN git clone https://github.com/robbyrussell/oh-my-zsh.git /root/.oh-my-zsh
ADD confs/zsh/zshrc /root/.zshrc

# Configure Tmux
ADD confs/tmux/tmux.conf /root/.tmux.conf

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
