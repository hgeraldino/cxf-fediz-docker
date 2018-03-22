# Apache Fediz Relying Party (RP) 
# https://cxf.apache.org/fediz.html
# Version:  0.1
FROM maven:3.5.2-jdk-8

# set work dir
WORKDIR	/fediz

# Download tomcat files
RUN     curl -sS https://www-us.apache.org/dist/tomcat/tomcat-7/v7.0.85/bin/apache-tomcat-7.0.85.zip > apache-tomcat-7.0.85.zip  && \
        curl -sS https://www-us.apache.org/dist/tomcat/tomcat-7/v7.0.85/bin/apache-tomcat-7.0.85.zip.sha1 > apache-tomcat-7.0.85.zip.sha1

# Download fediz files
RUN	curl -sS https://www-us.apache.org/dist/cxf/fediz/1.4.3/fediz-1.4.3-source-release.zip > apache-fediz-1.4.3.zip && \
	curl -sS https://www-us.apache.org/dist/cxf/fediz/1.4.3/fediz-1.4.3-source-release.zip.sha1 > apache-fediz-1.4.3.zip.sha1

# checksum & unzip
RUN	echo "`cat apache-fediz-1.4.3.zip.sha1`  apache-fediz-1.4.3.zip" | sha1sum -c - && \
	sha1sum -c apache-tomcat-7.0.85.zip.sha1

RUN	unzip apache-fediz-1.4.3.zip

RUN	unzip apache-tomcat-7.0.85.zip -d /usr/local && \
	ln -s /usr/local/apache-tomcat-7.0.85/ /usr/local/tomcat
		
# set work dir
WORKDIR	/fediz/fediz-1.4.3/examples

# compile 
RUN 	mvn package -DskipTests

# copy keystores to tomcat's ROOT
RUN     cp samplekeys/*.jks /usr/local/tomcat

# copy WAR files
RUN	cp spring2Webapp/target/fedizhelloworld.war /usr/local/tomcat/webapps/fedizhelloworld.war

# set work dir
WORKDIR /usr/local/tomcat

# copy server.xml 
COPY    server.xml conf/

# set file mode 
RUN	chmod +x bin/*.sh

# start rp
CMD ["./bin/catalina.sh", "run"]
