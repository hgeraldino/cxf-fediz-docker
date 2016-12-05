# Apache Fediz Identity Provider (IDp) 
# https://cxf.apache.org/fediz.html
# Version:  0.1
FROM tomcat:7.0

# set work dir
WORKDIR	/tmp

# Download binaries & checksums
RUN		curl -sS http://central.maven.org/maven2/org/hsqldb/hsqldb/2.3.4/hsqldb-2.3.4.jar > hsqldb-2.3.4.jar && \
		curl -sS http://central.maven.org/maven2/org/hsqldb/hsqldb/2.3.4/hsqldb-2.3.4.jar.md5 > hsqldb-2.3.4.jar.md5 && \
		curl -sS https://www-us.apache.org/dist/cxf/fediz/1.2.3/apache-fediz-1.2.3.zip > apache-fediz-1.2.3.zip && \
		curl -sS https://www-us.apache.org/dist/cxf/fediz/1.2.3/apache-fediz-1.2.3.zip.md5 > apache-fediz-1.2.3.zip.md5

# checksum & unzip
RUN		echo "`cat hsqldb-2.3.4.jar.md5`  hsqldb-2.3.4.jar" | md5sum -c - 

RUN		echo "`cat apache-fediz-1.2.3.zip.md5`  apache-fediz-1.2.3.zip" | md5sum -c - && \
		unzip apache-fediz-1.2.3.zip

# move hsqldb driver
RUN		mv hsqldb-2.3.4.jar	/usr/local/tomcat/lib/

# set work dir
WORKDIR	/tmp/apache-fediz-1.2.3/idp/

# move WAR files
RUN		mv war/fediz-idp-sts.war /usr/local/tomcat/webapps && \ 
		mv war/fediz-idp.war /usr/local/tomcat/webapps

# move keystores to tomcat's ROOT
RUN     mv src/idp/src/main/resources/idp-ssl-trust.jks /usr/local/tomcat && \ 
		mv src/idp/src/main/resources/idp-ssl-key.jks /usr/local/tomcat && \
		mv src/idp/src/main/resources/stsrealm_?.jks /usr/local/tomcat && \
		mv src/sts/src/main/resources/ststrust.jks /usr/local/tomcat

# copy server.xml 
WORKDIR /usr/local/tomcat

COPY    server.xml conf/

# start IDp
CMD ["catalina.sh", "run"]