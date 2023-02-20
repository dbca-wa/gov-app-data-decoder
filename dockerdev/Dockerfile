# Prepare the base environment.
FROM ubuntu:22.04 as builder_base_mobileapp
MAINTAINER asi@dbca.wa.gov.au
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Australia/Perth
ENV PRODUCTION_EMAIL=True
ENV SECRET_KEY="ThisisNotRealKey"
RUN apt-get clean
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install --no-install-recommends -y wget git rsyslog 
RUN apt-get install --no-install-recommends -y libpq-dev patch
RUN apt-get install --no-install-recommends -y ssh htop

WORKDIR /app
COPY requirements.txt ./
RUN rm -rf /var/lib/{apt,dpkg,cache,log}/ /tmp/* /var/tmp/*

COPY timezone /etc/timezone
ENV TZ=Australia/Perth
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

COPY startup.sh /
RUN chmod 755 /startup.sh
COPY gunicorn.ini ./
RUN touch /app/.env
COPY .git ./.git

# Install nodejs
RUN update-ca-certificates
# install node 16
RUN touch install_node.sh
RUN curl -fsSL https://deb.nodesource.com/setup_16.x -o install_node.sh
RUN chmod +x install_node.sh && ./install_node.sh
RUN apt-get install -y nodejs
# Install nodejs

EXPOSE 8080
CMD ["/bin/bash"]
