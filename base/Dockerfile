FROM rastasheep/ubuntu-sshd:14.04
MAINTAINER Simón Castillo, <scastb@gmail.com>

# Fix locales
RUN locale-gen en_US.UTF-8
RUN dpkg-reconfigure locales
RUN echo "LC_ALL=en_US.UTF-8" > /etc/environment
RUN echo "LANG=en_US.UTF-8" > /etc/environment

# Install some basic utilities.
RUN apt-get update
RUN apt-get dist-upgrade -y -q
RUN apt-get install -y -q software-properties-common command-not-found
RUN apt-get install -y -q build-essential tig man atool
RUN apt-get install -y -q zsh wget curl tmux htop aptitude psmisc
RUN apt-get install -y -q emacs24-nox nano vim python-pygments
RUN apt-get install -y -q git mercurial subversion atfs

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

# Go
RUN apt-get install -y golang

# Change to ZSH
RUN git clone git://github.com/robbyrussell/oh-my-zsh.git /root/.oh-my-zsh
ADD confs/zsh/zshrc /root/.zshrc
RUN chsh --shell $(which zsh)

# Configure Tmux
ADD confs/tmux/tmux.conf /root/.tmux.conf

CMD    ["/usr/sbin/sshd", "-D"]
