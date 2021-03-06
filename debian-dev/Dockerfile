#
#  Author: Hari Sekhon
#  Date: 2016-01-16 09:58:07 +0000 (Sat, 16 Jan 2016)
#
#  vim:ts=4:sts=4:sw=4:et
#
#  https://github.com/harisekhon/Dockerfiles
#
#  If you're using my code you're welcome to connect with me on LinkedIn and optionally send me feedback to help improve or steer this or other code I publish
#
#  https://www.linkedin.com/in/harisekhon
#

FROM harisekhon/debian-java:jdk7
MAINTAINER Hari Sekhon (https://www.linkedin.com/in/harisekhon)

LABEL Description="Debian Dev Build"

ENV GRADLE_HOME=/opt/gradle
ENV JYTHON_HOME=/opt/jython
ENV PATH $PATH:$GRADLE_HOME/bin:$JYTHON_HOME/bin

RUN apt-get update && \
    apt-get upgrade -y --no-install-recommends && \
    apt-get install -y --no-install-recommends \
                       build-essential \
                       curl \
                       dstat \
                       expect \
                       git \
                       #gradle \
                       groovy2 \
                       libdbd-mysql-perl \
                       libev4 \
                       libexpat1-dev \
                       libkrb5-dev \
                       libmysqlclient-dev \
                       libsasl2-dev \
                       libsnappy-dev \
                       libssl-dev \
                       make \
                       maven \
                       net-tools \
                       python-dev \
                       python-pip \
                       python-setuptools \
                       ruby \
                       ruby-dev \
                       # TODO: Scala 2.9, put newer scala on here
                       scala \
                       sysstat \
                       unzip \
                       vim \
                       wget \
                       zip && \
    # for sbt https repo
    apt-get install -y apt-transport-https && \
    echo "deb https://dl.bintray.com/sbt/debian /" | tee -a /etc/apt/sources.list.d/sbt.list && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 642AC823 && \
    apt-get update && \
    apt-get install -y sbt && \
    # for Maven 3
    #echo deb http://ppa.launchpad.net/natecarlson/maven3/ubuntu precise main >> /etc/apt/sources.list && \
    #apt-get update && \
    #apt-get install -y maven && \
    apt-get autoremove -y && \
    apt-get clean

# Gradle in Debian is old 1.5 :-(
RUN \
    cd /opt && \
    wget -t 100 --retry-connrefused https://services.gradle.org/distributions/gradle-2.13-bin.zip && \
    unzip gradle-2.13-bin.zip && \
    ln -sv gradle-2.13 gradle && \
    rm -f gradle-2.13-bin.zip

# Jython
COPY jython_autoinstall.exp /
RUN \
    cd / && \
    wget -O jython-installer.jar "http://search.maven.org/remotecontent?filepath=org/python/jython-installer/2.7.0/jython-installer-2.7.0.jar" && \
    ./jython_autoinstall.exp && \
    ln -sf /opt/jython-2.7.0 /opt/jython && \
    rm -fv jython-installer.jar && \
    rm -fv jython_autoinstall.exp

COPY profile.d/gradle.sh /etc/profile.d/
COPY profile.d/jython.sh /etc/profile.d/

CMD /bin/bash
