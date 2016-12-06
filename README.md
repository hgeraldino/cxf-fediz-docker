# cxf-fediz-docker
Docker images for Apache CXF Fediz IDP (WS-Federation), and a companion Spring RP web application.

Build
-----

To create the Identity Provider `fediz/idp`, execute the following command on the idp folder:

    docker build -t fediz/idp .

To create the Relying Party (RP) image, execute the following command on the rp folder:

    docker build -t fediz/rp .


Run
-----
To run the IDP image (and bind to port):

    docker run -it -p 9443:9443 fediz/idp

To run the RP image: 

	docker run -it -p 8443:8443 fediz/rp

docker-compose
--------------

A docker-compose.yml file is included, and it will start both the IDp and RP tomcat instances (instead of issuing separate commands to build and run each image). If docker-compose is installed on the system, just run:

	docker-compose build && docker-compose up


Usage
---
To access the Relying Party application, browse to:

	  https://localhost:8443/fedizhelloworld/secure/fedservlet

The request will be immediately redirected to the IDP site, where you can log in on any of the available realms. 

For information about the provisioned accounts check https://cxf.apache.org/fediz-idp-11.html

For general information about the WS-Federation architecture and Apache CXF Fediz in general, please visit the Apache CXF Fediz website.
