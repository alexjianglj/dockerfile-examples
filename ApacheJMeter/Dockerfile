######################## Dockerfile for Apache JMeter version 3.3 #########################################
#
# This Dockerfile builds a basic installation of Apache JMeter.
#
# The Apache JMeter application is open source software, a 100% pure JAVA application
# designed to load test functional behaviour and measure performance. It was originally
# designed for testing Web Applications but has since expanded to other test functions.
#
# To build this image, from the directory containing this Dockerfile
# (assuming that the file is named Dockerfile):
# docker build -t <image_name> .
#
# To create a container of Apache JMeter image run the below command:
# docker run --name <container_name> -it  <image_name>
#
# To test a JMX file use the following command:
# docker run --rm=true -v /path/to/JMX_file/filename.jmx:/filename.jmx jmeter_imagename /filename.jmx
#
#########################################################################################################

# Base image
FROM s390x/ubuntu:16.04

# The author
MAINTAINER LoZ Open Source Ecosystem (https://www.ibm.com/developerworks/community/groups/community/lozopensource)

ENV SOURCE_DIR=/root
WORKDIR $SOURCE_DIR

# Set Environment varibles
ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-s390x
ENV PATH=$JAVA_HOME/bin:$PATH

# Install dependencies
RUN apt-get update &&  apt-get install -y \
    ant \
    openjdk-8-jdk \
    openssl \
    git \
	openjfx \

# Download from git and build
 && git clone git://git.apache.org/jmeter.git \
 && cd $SOURCE_DIR/jmeter && git checkout v3_3 \
 && cd $SOURCE_DIR/jmeter && ant download_jars && ant \
 && cp -r $SOURCE_DIR/jmeter /opt/apache-jmeter-3.3 \

#Remove G1GC algorithm as it is unimplemented in OpenJDK zero
 && sed -i '93d' /opt/apache-jmeter-3.3/bin/jmeter \
 
# Clean the Cache data and remove source files and dependencies
 && rm -rf $SOURCE_DIR \
 && apt-get remove -y \
    ant \
    git \
 && apt-get autoremove -y \
 && apt-get clean && rm -rf /var/lib/apt/lists/*

# Set Environment Variable for JMeter
ENV PATH $PATH:/opt/apache-jmeter-3.3/bin

ENTRYPOINT ["jmeter","-n","-t"]

# End Of Dockerfile
