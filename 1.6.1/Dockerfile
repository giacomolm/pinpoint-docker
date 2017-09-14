FROM debian:jessie
MAINTAINER Giacomo Lamonaco <giacomo.lamonaco@yahoo.it>

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
  apt-utils \
  curl \
  git \
  net-tools \
  procps \
  software-properties-common \
  wget

RUN echo "deb http://http.debian.net/debian jessie-backports main" > /etc/apt/sources.list.d/jessie-backports.list

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y -t jessie-backports openjdk-8-jdk ca-certificates-java
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64

RUN useradd pinpoint-hippo -m

WORKDIR /usr/local/apache-maven
ADD https://archive.apache.org/dist/maven/maven-3/3.2.5/binaries/apache-maven-3.2.5-bin.tar.gz ./
ADD https://archive.apache.org/dist/maven/maven-3/3.2.5/binaries/apache-maven-3.2.5-bin.tar.gz.sha1 ./
RUN [ $(sha1sum apache-maven-3.2.5-bin.tar.gz | grep --only-matching -m 1 '^[0-9a-f]*') = $(cat apache-maven-3.2.5-bin.tar.gz.sha1) ]
RUN tar -xf apache-maven-3.2.5-bin.tar.gz
ENV PATH $PATH:/usr/local/apache-maven/apache-maven-3.2.5/bin
RUN rm apache-maven-3.2.5-bin.tar.gz apache-maven-3.2.5-bin.tar.gz.sha1

WORKDIR /usr/local/apache-tomcat
ADD http://archive.apache.org/dist/tomcat/tomcat-8/v8.0.46/bin/apache-tomcat-8.0.46.tar.gz ./
RUN tar -xf apache-tomcat-8.0.46.tar.gz
RUN rm apache-tomcat-8.0.46.tar.gz

RUN git clone https://github.com/naver/pinpoint.git /pinpoint-hippo/pinpoint
WORKDIR /pinpoint-hippo/pinpoint
RUN git checkout tags/1.6.1

WORKDIR /pinpoint-hippo/1.6.1
ADD https://github.com/naver/pinpoint/releases/download/1.6.1/pinpoint-agent-1.6.1.tar.gz ./
ADD https://github.com/naver/pinpoint/releases/download/1.6.1/pinpoint-collector-1.6.1.war ./
ADD https://github.com/naver/pinpoint/releases/download/1.6.1/pinpoint-web-1.6.1.war ./
RUN tar -xf pinpoint-agent-1.6.1.tar.gz
RUN rm -rf /usr/local/apache-tomcat/apache-tomcat-8.0.46/webapps/ROOT/
RUN cp pinpoint-collector-1.6.1.war /usr/local/apache-tomcat/apache-tomcat-8.0.46/webapps
RUN cp pinpoint-web-1.6.1.war /usr/local/apache-tomcat/apache-tomcat-8.0.46/webapps/ROOT.war

WORKDIR /pinpoint-hippo/hbase
ADD https://archive.apache.org/dist/hbase/hbase-1.0.3/hbase-1.0.3-bin.tar.gz ./
RUN tar -xf hbase-1.0.3-bin.tar.gz
RUN rm hbase-1.0.3-bin.tar.gz
RUN ln -s hbase-1.0.3 hbase

WORKDIR /pinpoint-hippo
VOLUME [/pinpoint-hippo]
