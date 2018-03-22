# Apache Fediz Identity Provider (IDp) 
# https://cxf.apache.org/fediz.html
# Version:  0.1
#
# Dependencies: md5sum
# On OSX: `brew install md5sha1sum`
FROM maven:3.5.2-jdk-8
  
# Install JDK & maven
RUN apt-get update

# set work dir
WORKDIR	/fediz

# Download binaries & checksums
RUN     curl -sS https://www-us.apache.org/dist/tomcat/tomcat-7/v7.0.85/bin/apache-tomcat-7.0.85.zip > apache-tomcat-7.0.85.zip  && \
        curl -sS https://www-us.apache.org/dist/tomcat/tomcat-7/v7.0.85/bin/apache-tomcat-7.0.85.zip.sha1 > apache-tomcat-7.0.85.zip.sha1
		
RUN	curl -sS http://central.maven.org/maven2/org/hsqldb/hsqldb/2.3.5/hsqldb-2.3.5.jar > hsqldb-2.3.5.jar && \
	curl -sS http://central.maven.org/maven2/org/hsqldb/hsqldb/2.3.5/hsqldb-2.3.5.jar.sha1 > hsqldb-2.3.5.jar.sha1
		
RUN	curl -sS https://www-us.apache.org/dist/cxf/fediz/1.4.3/fediz-1.4.3-source-release.zip > apache-fediz-1.4.3.zip && \
	curl -sS https://www-us.apache.org/dist/cxf/fediz/1.4.3/fediz-1.4.3-source-release.zip.sha1 > apache-fediz-1.4.3.zip.sha1

# checksum
RUN     sha1sum -c apache-tomcat-7.0.85.zip.sha1
RUN	echo "`cat hsqldb-2.3.5.jar.sha1` hsqldb-2.3.5.jar" | sha1sum -c -
RUN     echo "`cat apache-fediz-1.4.3.zip.sha1` apache-fediz-1.4.3.zip" | sha1sum -c -

# unzip
RUN   	unzip apache-fediz-1.4.3.zip

RUN     unzip apache-tomcat-7.0.85.zip -d /usr/local
RUN     ln -s /usr/local/apache-tomcat-7.0.85/ /usr/local/tomcat

# move hsqldb driver
RUN		mv hsqldb-2.3.5.jar	/usr/local/tomcat/lib/

# set work dir
WORKDIR	/fediz/fediz-1.4.3

# build project
ENV     MAVEN_OPTS -Xmx1024m
RUN	mvn package -DskipTests=true -pl services/sts -pl services/idp

# move WAR files
RUN	mv services/sts/target/fediz-idp-sts.war /usr/local/tomcat/webapps && \ 
	mv services/idp/target/fediz-idp.war /usr/local/tomcat/webapps

# move keystores to tomcat's ROOT
WORKDIR	/fediz/fediz-1.4.3/services/idp/src/main/resources

RUN     mv idp-ssl-trust.jks /usr/local/tomcat && \ 
	mv idp-ssl-key.jks /usr/local/tomcat && \
	mv stsrealm_?.jks /usr/local/tomcat

WORKDIR	/fediz/fediz-1.4.3/services/sts/src/main/resources
		
RUN	mv ststrust.jks /usr/local/tomcat

# copy server.xml 
WORKDIR /usr/local/tomcat
COPY    server.xml conf/

# start IDp
RUN		chmod +x bin/*.sh
CMD ["./bin/catalina.sh", "run"]
