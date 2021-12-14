#FROM openjdk:18-slim
FROM bitnami/jenkins:2.303.3

# JMeter version
ARG JMETER_VERSION=5.4.1

# Install JMeter
RUN   mkdir /opt/bitnami/jmeter
COPY ./apache-jmeter-$JMETER_VERSION.tgz /opt/bitnami/jmeter
RUN   cd /opt/bitnami/jmeter/ \
      && tar -xzf apache-jmeter-$JMETER_VERSION.tgz \
      && rm apache-jmeter-$JMETER_VERSION.tgz

# Set JMeter Home
ENV JMETER_HOME /opt/bitnami/jmeter/apache-jmeter-$JMETER_VERSION

# Add JMeter to the Path
ENV PATH $JMETER_HOME/bin:$PATH