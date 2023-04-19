FROM openjdk:8-jre-slim
MAINTAINER "tom " <3299216138@qq.com>

# Create directory structure, download Tigase XMPP Server -dist-max package and unpack it
# At the same time remove setting JAVA_HOME from etc/tigase.conf as variable is already set
# and add additional parameters to GC variable to make Java aware of Docker limits
# Then move content of etc/ directory to etc.orig/ to be able to use volume to host configuration.
# Additionally we are removing docs/ and downloaded package to reduce size.
RUN mkdir /home/tigase \
	&& cd /home/tigase \
	&& apt-get update \
	&& apt-get install -y --no-install-recommends wget \
	&& wget https://github.com/tigase/tigase-server/releases/download/tigase-server-7.1.3/tigase-server-7.1.3-b4482-dist-max.tar.gz \
	&& apt-get remove -y wget \
	&& rm -rf /var/lib/apt/lists/* \
	&& tar -zxvf tigase-server-7.1.3-b4482-dist-max.tar.gz \
	&& ln -s `find . -name 'tigase-server-*' -maxdepth 1 -type d` tigase-server \
	&& rm -rf tigase-server/docs/ \
	&& rm -rf tigase-server-7.1.3-b4482-dist-max.tar.gz \
	&& sed -i -e 's/JAVA_HOME/#JAVA_HOME/;/^JAVA_METASPACE=.*/a \\nGC="${GC} -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap "\n' tigase-server/etc/tigase.conf \
	&& mv tigase-server/etc tigase-server/etc.orig

# Add statup scripts and make them executable
# We need custom scripts to make sure that database is upgraded
# and that Tigase will NOT be launched as background service!
RUN chmod 755 /home/tigase/tigase-server/*.sh
RUN chmod 755 home/tigase/tigase-server/scripts/*.sh

# Set TIGASE_HOME variable
ENV TIGASE_HOME /home/tigase/tigase-server

# Set ports of the container which can be exposed
EXPOSE 5222 5223  5290  9080 9090

# Set possible volume (directories which can be mounted from the docker host machine)
# like etc/, conf/, certs/, logs/, data/
VOLUME ["/home/tigase/tigase-server/etc", "/home/tigase/tigase-server/conf", "/home/tigase/tigase-server/certs", "/home/tigase/tigase-server/logs", "/home/tigase/tigase-server/data"]

# Define script which should be executed on container startup
CMD ["/bin/bash", "/home/tigase/tigase-server/start.sh"]
