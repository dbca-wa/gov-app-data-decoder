# Prepare the base environment.
FROM ubuntu:22.04 as builder_base_mobileapp
MAINTAINER asi@dbca.wa.gov.au
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Australia/Perth
ENV PRODUCTION_EMAIL=True
ENV SECRET_KEY="ThisisNotRealKey"
ENV ANDROID_HOME=/usr/lib/android-sdk
ENV ANDROID_SDK_ROOT=/usr/lib/android-sdk
ENV PATH=$PATH:$ANDROID_SDK_ROOT/cmdline-tools/bin
ENV PATH=$PATH:$ANDROID_SDK_ROOT/cmdline-tools/bin 

RUN apt-get clean
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install --no-install-recommends -y wget git rsyslog curl ca-certificates
RUN apt-get install --no-install-recommends -y libpq-dev patch
RUN apt-get install --no-install-recommends -y ssh htop 

WORKDIR /app
RUN rm -rf /var/lib/{apt,dpkg,cache,log}/ /tmp/* /var/tmp/*

ENV TZ=Australia/Perth
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN touch /app/.env
#COPY ./packages/commandlinetools-linux-9477386_latest.zip $ANDROID_SDK_ROOT
RUN cd $ANDROID_SDK_ROOT && wget https://dl.google.com/android/repository/commandlinetools-win-9477386_latest.zip
RUN cd $ANDROID_SDK_ROOT && unzip commandlinetools-linux-9477386_latest.zip
#COPY .git ./.git

## Install nodejs
RUN update-ca-certificates
# install node 16
RUN touch install_node.sh
RUN curl -fsSL https://deb.nodesource.com/setup_16.x -o install_node.sh
RUN chmod +x install_node.sh && ./install_node.sh
RUN apt-get install -y nodejs
# Install nodejs

# Development Defaults
RUN apt-get install --no-install-recommends -y patch
RUN apt-get install --no-install-recommends -y libpq-dev git ssh sudo 
RUN apt-get install --no-install-recommends -y screen
RUN apt-get install --no-install-recommends -y virtualenv
RUN apt-get install --no-install-recommends -y graphviz
RUN apt-get install --no-install-recommends -y python3-pydot
RUN apt-get install --no-install-recommends -y default-jdk default-jre android-sdk google-android-ndk-installerd

RUN npm install -g expo-cli
RUN npm install -g eas-cli

RUN openssl req -new -newkey rsa:4096 -days 3650 -nodes -x509 -subj  "/C=AU/ST=Western Australia/L=Perth/O=Development Environment/OU=IT Department/CN=example.com"  -keyout /etc/ssl/private/selfsignedssl.key -out /etc/ssl/private/selfsignedssl.crt

RUN echo 'if ! [ -n "${SUDO_USER}" -a -n "${SUDO_PS1}" ]; then' >> /etc/bash.bashrc
RUN echo "export PS1='\e[0;31mdev:\e[m \e[0;32m\${debian_chroot:+(\$debian_chroot)}\u@\h:\w\$\e[m '" >> /etc/bash.bashrc
RUN echo "fi" >> /etc/bash.bashrc

# For windows 
RUN groupadd -g 1000 docker
RUN useradd -g 1000 -u 10000 docker -s /bin/bash
RUN mkdir /home/docker
RUN chown docker.docker /home/docker
RUN cp /root/.bashrc /home/docker
RUN cp /root/.profile /home/docker

# For Linux
RUN groupadd -g 20000 container
RUN useradd -g 20000 -u 200000 container -s /bin/bash
RUN mkdir /home/container
RUN chown container.container /home/container
RUN cp /root/.bashrc /home/container
RUN cp /root/.profile /home/container

# Development Defaults


EXPOSE 8080
CMD ["/bin/bash"]
