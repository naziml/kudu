FROM buildpack-deps:jessie-curl
MAINTAINER Nazim Lala <naziml@microsoft.com>

ENV DEBIAN_FRONTEND noninteractive

COPY package/Kudu/Kudu.zip /tmp
COPY package/Kudu/apache2.conf /tmp
COPY package/Kudu/kudu.conf /tmp
COPY package/Kudu/node-v4.4.7-linux-x64.tar.xz /tmp
COPY package/Kudu/node-v4.5.0-linux-x64.tar.xz /tmp
COPY package/Kudu/node-v6.2.2-linux-x64.tar.xz /tmp
COPY package/Kudu/node-v6.6.0-linux-x64.tar.xz /tmp
COPY package/Kudu/npm-2.15.8.zip /tmp
COPY package/Kudu/npm-2.15.9.zip /tmp
COPY package/Kudu/npm-3.9.5.zip /tmp
COPY package/Kudu/npm-3.10.3.zip /tmp

# Install gpg keys for mono
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 \
  --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF

RUN echo "deb http://download.mono-project.com/repo/debian wheezy main" > \
  /etc/apt/sources.list.d/mono-xamarin.list \
  && echo "deb http://download.mono-project.com/repo/debian wheezy-apache24-compat main" >> \
  /etc/apt/sources.list.d/mono-xamarin.list \
  && echo "deb http://download.mono-project.com/repo/debian wheezy-libjpeg62-compat main" >> \
  /etc/apt/sources.list.d/mono-xamarin.list \
  && apt-get update \
  && apt-get install -y apt-utils --no-install-recommends \
  && apt-get install -y unzip --no-install-recommends \
  && apt-get install -y xz-utils --no-install-recommends \
  && apt-get install -y mono-complete --no-install-recommends \
  && apt-get install -y mono-apache-server4 --no-install-recommends \
  && apt-get install -y libapache2-mod-mono --no-install-recommends \
  && apt-get install -y git --no-install-recommends \
  && apt-get install -y openssh-client --no-install-recommends \
  && apt-get install -y tcptraceroute \
  && wget -O /usr/bin/tcpping http://www.vdberg.org/~richard/tcpping \
  && chmod 755 /usr/bin/tcpping

# Install npm package
RUN mkdir -p /opt/npm/2.15.8/node_modules \
  && unzip -q /tmp/npm-2.15.8.zip -d /opt/npm/2.15.8/node_modules \
  && mv /opt/npm/2.15.8/node_modules/npm-2.15.8 /opt/npm/2.15.8/node_modules/npm \
  && ln -s /opt/npm/2.15.8/node_modules/npm/bin/npm /opt/npm/2.15.8/npm \
  && chmod -R 755 /opt/npm/2.15.8/node_modules/npm/bin

RUN mkdir -p /opt/npm/2.15.9/node_modules \
  && unzip -q /tmp/npm-2.15.9.zip -d /opt/npm/2.15.9/node_modules \
  && mv /opt/npm/2.15.9/node_modules/npm-2.15.9 /opt/npm/2.15.9/node_modules/npm \
  && ln -s /opt/npm/2.15.9/node_modules/npm/bin/npm /opt/npm/2.15.9/npm \
  && chmod -R 755 /opt/npm/2.15.9/node_modules/npm/bin

RUN mkdir -p /opt/npm/3.9.5/node_modules \
  && unzip -q /tmp/npm-3.9.5.zip -d /opt/npm/3.9.5/node_modules \
  && mv /opt/npm/3.9.5/node_modules/npm-3.9.5 /opt/npm/3.9.5/node_modules/npm \
  && ln -s /opt/npm/3.9.5/node_modules/npm/bin/npm /opt/npm/3.9.5/npm \
  && chmod -R 755 /opt/npm/3.9.5/node_modules/npm/bin

RUN mkdir -p /opt/npm/3.10.3/node_modules \
  && unzip -q /tmp/npm-3.10.3.zip -d /opt/npm/3.10.3/node_modules \
  && mv /opt/npm/3.10.3/node_modules/npm-3.10.3 /opt/npm/3.10.3/node_modules/npm \
  && ln -s /opt/npm/3.10.3/node_modules/npm/bin/npm /opt/npm/3.10.3/npm \
  && chmod -R 755 /opt/npm/3.10.3/node_modules/npm/bin


RUN chown -R root:root /opt/npm
  
# Install nodejs package 
RUN mkdir -p /opt/nodejs \
  && tar xfJ /tmp/node-v4.4.7-linux-x64.tar.xz -C /opt/nodejs \
  && rm -f node-v4.4.7-linux-x64.tar.xz \
  && mv /opt/nodejs/node-v4.4.7-linux-x64 /opt/nodejs/4.4.7 \
  && echo "2.15.8" > /opt/nodejs/4.4.7/npm.txt 

RUN mkdir -p /opt/nodejs \
  && tar xfJ /tmp/node-v4.5.0-linux-x64.tar.xz -C /opt/nodejs \
  && rm -f node-v4.5.0-linux-x64.tar.xz \
  && mv /opt/nodejs/node-v4.5.0-linux-x64 /opt/nodejs/4.5.0 \
  && echo "2.15.9" > /opt/nodejs/4.5.0/npm.txt 

RUN tar xfJ /tmp/node-v6.2.2-linux-x64.tar.xz -C /opt/nodejs \
  && rm -f node-v6.2.2-linux-x64.tar.xz \
  && mv /opt/nodejs/node-v6.2.2-linux-x64 /opt/nodejs/6.2.2 \
  && echo "3.9.5" > /opt/nodejs/6.2.2/npm.txt

RUN tar xfJ /tmp/node-v6.6.0-linux-x64.tar.xz -C /opt/nodejs \
  && rm -f node-v6.6.0-linux-x64.tar.xz \
  && mv /opt/nodejs/node-v6.6.0-linux-x64 /opt/nodejs/6.6.0 \
  && echo "3.10.3" > /opt/nodejs/6.6.0/npm.txt

RUN chown -R root:root /opt/nodejs
  
# Set default node (4.5) and npm (2.15.9)
#   register to be default node version  
RUN ln -s /opt/nodejs/4.5.0/bin/node /opt/nodejs/node \
  && ln -s /opt/nodejs/4.5.0/npm.txt /opt/nodejs/npm.txt \
# enable node to be global command
  && rm -f /usr/bin/node \
  && ln -s /opt/nodejs/4.5.0/bin/node /usr/bin/node \
  && rm -f /opt/nodejs/4.5.0/bin/npm

# export bin folder to path so that module install with -g from npm will be avalable in global
# this need to be done in children image
# ENV PATH $PATH:/opt/nodejs/4.5.0/bin
  
RUN ln -s /opt/npm/2.15.9/node_modules/npm/bin/npm /opt/nodejs/npm \
  && ln -s /opt/npm/2.15.9/node_modules /opt/nodejs/node_modules \
# enable default npm to be global command
  && rm -rf /usr/bin/npm \
  && ln -s /opt/npm/2.15.9/node_modules/npm/bin/npm /usr/bin/npm \
  && ln -s /opt/npm/2.15.9/node_modules /usr/bin/node_modules
  
# Install Kudu
RUN npm install -g kuduscript \
  && npm install -g kudusync \
  && cp /tmp/kudu.conf /etc/apache2/sites-available/kudu.conf \
  && cp /tmp/apache2.conf /etc/apache2/apache2.conf  \
  && unzip -q -o /tmp/Kudu.zip \
  && cp -rf ./SiteExtensions/* /opt \
  && rm -f /tmp/Kudu.zip \
  && rm -f /tmp/apache2.conf \
  && rm -f /tmp/kudu.conf \
  && rm -rf ./SiteExtensions \
  && cat /opt/Kudu/Web.config | \
  sed 's|  <location path="." inheritInChildApplications="false">|  <location path="~/../../../opt/Kudu" inheritInChildApplications="false">|' \
  > /opt/Kudu/Web.config2 \
  && mv /opt/Kudu/Web.config2 /opt/Kudu/Web.config \
  && chmod 755 /opt/Kudu/bin/kudu.exe \
  && chmod 755 /opt/Kudu/bin/node_modules/.bin/kuduscript \
  && mkdir -p /opt/Kudu/local \
  && chmod 755 /opt/Kudu/local

# Install basic utils
RUN apt-get install -y vim tree --no-install-recommends

# Upgrade mono
RUN echo "deb http://download.mono-project.com/repo/debian beta/snapshots/c7-ms/. main" | tee /etc/apt/sources.list.d/mono-xamarin-c7-ms.list \
  && apt-get update \
  && apt-get install -y libmono-system-net-http-webrequest4.0-cil --no-install-recommends
