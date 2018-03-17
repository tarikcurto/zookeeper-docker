FROM tarikcurto/centos-java:7-8

ENV ZOOKEEPER_VERSION 3.4.11

# Script files
COPY zookeeper-download.sh /tmp/

# Download necessary packages
RUN yum install -y epel-release && yum install -y wget jq openssh-server

# Download Zookeeper & keys
RUN chmod +x /tmp/zookeeper-download.sh && /tmp/zookeeper-download.sh

# Verify download
RUN wget -q https://www.apache.org/dist/zookeeper/zookeeper-${ZOOKEEPER_VERSION}/zookeeper-${ZOOKEEPER_VERSION}.tar.gz.md5 -O /tmp/zookeeper-${ZOOKEEPER_VERSION}.tar.gz.md5 && \
    cd /tmp && md5sum -c /tmp/zookeeper-${ZOOKEEPER_VERSION}.tar.gz.md5

# Install Zookeeper folder & delete compressed file
RUN tar -xzf /tmp/zookeeper-${ZOOKEEPER_VERSION}.tar.gz -C /opt

# Delete uncessary files
RUN rm -f /tmp/zookeeper-${ZOOKEEPER_VERSION}.tar.gz

# Configure
ENV ZK_HOME /opt/zookeeper-${ZOOKEEPER_VERSION}
COPY zoo.cfg /opt/zookeeper-${ZOOKEEPER_VERSION}/conf/
COPY zookeeper-start.sh /usr/bin/

## Replace zookeper data directory
#RUN mkdir $ZK_HOME/data && \
#    sed  -i "s|dataDir=/tmp/zookeeper|dataDir=$ZK_HOME/data|g" $ZK_HOME/conf/zoo.cfg

## Allow zookeper auto purge
#RUN sed -i -r 's|#autopurge|autopurge|g' $ZK_HOME/conf/zoo.cfg

EXPOSE 2181 2888 3888
WORKDIR /opt/zookeeper-${ZOOKEEPER_VERSION}
VOLUME ["/opt/zookeeper-${ZOOKEEPER_VERSION}/conf", "/opt/zookeeper-${ZOOKEEPER_VERSION}/data"]

CMD bash /usr/bin/zookeeper-start.sh