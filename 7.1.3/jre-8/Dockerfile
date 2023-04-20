FROM openjdk:8-jdk-alpine
MAINTAINER "tom " <3299216138@qq.com>
VOLUME  /opt/logs
ADD ./tigase-server-7.1.3-b4482/  /opt/tigase-server
RUN chmod  -R 755 /opt
EXPOSE 5222
EXPOSE 5280
ENTRYPOINT cd /opt/tigase-server; java -version; sh scripts/tigase.sh run etc/tigase.conf; wait $!