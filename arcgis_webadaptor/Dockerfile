FROM tomcat:9.0.56-jre8-openjdk-slim

MAINTAINER David.Ocean <2735403137@qq.com>

LABEL version="arcgis webadaptor 10.7" \
	  author="David.Ocean" \
	  description="This is for David.Ocean"


# Change from port 8080 to port 80.
RUN sed -i "s/8080/80/" /usr/local/tomcat/conf/server.xml
# Remove the redirect
RUN sed -i "s/redirectPort=\"8443\"//g" /usr/local/tomcat/conf/server.xml


# Create and install a self-signed certificate.
RUN keytool -genkey -alias tomcat -keyalg RSA -keystore /home/tomcat.keystore \
    -storepass admin123 -keypass admin123 \
    -dname "CN=David Ocean, OU=xykh Co, O=DavidOcean Fans Club, L=Hai Dian, ST=BeiJing, C=CN"
# Modify server.xml to activate the TLS service
RUN sed -i "s/<Service name=\"Catalina\">/<Service name=\"Catalina\">\n    <Connector port=\"443\" maxThreads=\"200\" scheme=\"https\" secure=\"true\" SSLEnabled=\"true\" keystorePass=\"admin123\" clientAuth=\"false\" sslProtocol=\"TLS\" keystoreFile=\"\/home\/tomcat.keystore\" \/>/" \
        /usr/local/tomcat/conf/server.xml

# Close the Tomcat
RUN /usr/local/tomcat/bin/shutdown.sh

# Update apt-get and instal some tools
RUN apt-get update && apt install -y iputils-ping vim

# start for arcgis webadaptor
COPY ./* /tmp/

EXPOSE 80 443

RUN tar xvzf /tmp/Web_Adaptor_Java_Linux_107_*.tar.gz -C /tmp/ && \
    /tmp/WebAdaptor/Setup -m silent -l yes -d /

RUN rm /tmp/Web_Adaptor_Java_Linux_107_*.tar.gz && \
    rm -rf /tmp/WebAdaptor



RUN cp /arcgis/webadaptor10.7/java/arcgis.war /usr/local/tomcat/webapps/



CMD /usr/local/tomcat/bin/startup.sh && tail -f /usr/local/tomcat/logs/catalina.out