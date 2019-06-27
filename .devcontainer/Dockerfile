FROM debian:9.9

LABEL maintainer="alitari67@gmail.com"

# Configure apt
ENV DEBIAN_FRONTEND=noninteractive

# install the tooks i wish to use
RUN apt-get update && \
  apt-get install -y sudo \
  curl \
  git-core \
  gnupg \
  linuxbrew-wrapper \
  locales \
  wget \
  # set up locale
  && locale-gen en_US.UTF-8

RUN echo 'deb http://download.opensuse.org/repositories/shells:/fish:/release:/2/Debian_9.0/ /' | sudo tee -a /etc/apt/sources.list
RUN wget -q -O - https://download.opensuse.org/repositories/shells:fish:release:2/Debian_9.0/Release.key | sudo apt-key add -

# install fish
RUN apt-get update && \
    apt-get install -y fish

ENV DEBIAN_FRONTEND=dialog

ENV USER_NAME alitari
ENV USER_PASSWORD password

# add a user (--disabled-password: the user won't be able to use the account until the password is set)
RUN adduser --quiet --disabled-password --shell /bin/zsh --home /home/$USER_NAME --gecos "User" $USER_NAME
# update the password
RUN echo "${USER_NAME}:${USER_PASSWORD}" | chpasswd && usermod -aG sudo $USER_NAME
RUN echo $USER_NAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USER_NAME && chmod 0440 /etc/sudoers.d/$USER_NAME

# set home
ENV HOME /home/$USER_NAME

RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && /usr/sbin/locale-gen

ENV TERM xterm

# the user we're applying this too (otherwise it most likely install for root)
USER $USER_NAME

# Set the default shell to fish rather than sh
ENV SHELL /usr/bin/fish

ADD init.fish ${HOME}/.config/omf

# install oh-my-fish with ocean theme
RUN cd $HOME && git clone https://github.com/oh-my-fish/oh-my-fish && \
  cd oh-my-fish && bin/install --offline --noninteractive --yes