# Apache Fediz Relying Party (RP) 
# https://cxf.apache.org/fediz.html
# Version:  0.1
FROM maven:3.3.9-jdk-7

# set work dir
WORKDIR	/tmp

# Download tomcat files
RUN     curl -sS https://www-us.apache.org/dist/tomcat/tomcat-7/v7.0.73/bin/apache-tomcat-7.0.73.zip > apache-tomcat-7.0.73.zip  && \
        curl -sS https://www-us.apache.org/dist/tomcat/tomcat-7/v7.0.73/bin/apache-tomcat-7.0.73.zip.md5 > apache-tomcat-7.0.73.zip.md5

# Download fediz files
RUN     curl -sS https://www-us.apache.org/dist/cxf/fediz/1.2.3/apache-fediz-1.2.3.zip > apache-fediz-1.2.3.zip && \
        curl -sS https://www-us.apache.org/dist/cxf/fediz/1.2.3/apache-fediz-1.2.3.zip.md5 > apache-fediz-1.2.3.zip.md5

# checksum & unzip
RUN		echo "`cat apache-fediz-1.2.3.zip.md5`  apache-fediz-1.2.3.zip" | md5sum -c - && \
		md5sum -c apache-tomcat-7.0.73.zip.md5 && \
		unzip apache-fediz-1.2.3.zip && \
		unzip apache-tomcat-7.0.73.zip -d /usr/local && \
		ln -s /usr/local/apache-tomcat-7.0.73/ /usr/local/tomcat

# set work dir
WORKDIR	/tmp/apache-fediz-1.2.3/examples

# compile 
RUN 	mvn clean package -T 0.5C

# copy keystores to tomcat's ROOT
RUN     cp samplekeys/rp-ssl-key.jks /usr/local/tomcat

# copy WAR files
RUN		cp spring2Webapp/target/fedizhelloworld.war /usr/local/tomcat/webapps/fedizhelloworld.war

# set work dir
WORKDIR /usr/local/tomcat

# copy server.xml 
COPY    server.xml conf/

# set file mode 
RUN		chmod +x bin/*.sh

# start rp
CMD ["./bin/catalina.sh", "run"]
