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

#FROM harisekhon/centos-java:jre7
# > 100MB smaller than centos
FROM harisekhon/debian-java:jre7
MAINTAINER Hari Sekhon (https://www.linkedin.com/in/harisekhon)

ARG SPARK_VERSION=1.6.1
ARG HADOOP_VERSION=2.6
ARG TAR=spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz

ENV PATH $PATH:/spark/bin

LABEL Description="Apache Spark" \
      "Spark Version"="$SPARK_VERSION"

#COPY spark-$SPARK_VERSION-bin-hadoop$HADOOP_VERSION /spark
#ADD "http://d3kbcqa49mib13.cloudfront.net/${TAR}"

RUN \
    apt-get update && \
    apt-get install -y --no-install-recommends sudo openssh-server wget tar && \
    wget -t 100 --retry-connrefused -O "${TAR}" "http://d3kbcqa49mib13.cloudfront.net/${TAR}" && \
    tar zxvf "${TAR}" && \
    rm -fv "${TAR}" && \
    ln -s "spark-$SPARK_VERSION-bin-hadoop$HADOOP_VERSION" spark && \
    # attempting to remove tar causes unmet dependency breakage
    apt-get purge -y wget && \
    apt-get autoremove -y && \
    apt-get clean

RUN mkdir /var/run/sshd && chmod 0755 /var/run/sshd && \
cp -v /spark/conf/spark-env.sh.template /spark/conf/spark-env.sh

COPY entrypoint.sh /

EXPOSE 4040 7077 8080 8081

#CMD /spark/bin/spark-shell
ENTRYPOINT ["/entrypoint.sh"]
