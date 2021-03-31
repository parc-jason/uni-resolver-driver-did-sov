# Dockerfile for universalresolver/driver-did-sov

FROM ubuntu:18.04
MAINTAINER Markus Sabadello <markus@danubetech.com>

USER root

RUN apt-get -y update && \
    apt-get install -y --no-install-recommends openjdk-11-jdk-headless maven git wget && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV JAVA_HOME /usr/lib/jvm/java-11-openjdk-amd64/
ENV PATH $JAVA_HOME/bin:$PATH

# install dependencies

RUN apt-get -y update && \
    apt-get install -y --no-install-recommends software-properties-common gnupg && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 68DB5E88 && \
    add-apt-repository "deb https://repo.sovrin.org/sdk/deb xenial stable" && \
    apt-get install -y libindy=1.14.0 && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Download jetty
RUN wget https://repo1.maven.org/maven2/org/eclipse/jetty/jetty-distribution/9.4.15.v20190215/jetty-distribution-9.4.15.v20190215.tar.gz
RUN tar xvf jetty-distribution-9.4.15.v20190215.tar.gz

# RUN mvn org.apache.maven.plugins:maven-dependency-plugin:3.1.1:get -DrepoUrl=https://repo.maven.apache.org/maven2 -Dartifact=org.eclipse.jetty:jetty-maven-plugin:9.4.18.v20190429
# RUN mvn org.apache.maven.plugins:maven-dependency-plugin:3.1.1:get -DrepoUrl=https://repo.maven.apache.org/maven2 -Dartifact=org.apache.maven.plugins:maven-war-plugin:3.2.2

# build driver-did-sov

ADD . /opt/driver-did-sov
RUN cd /opt/driver-did-sov && \
  mvn clean install package -N -DskipTests -P build-war && \
  cp /opt/driver-did-sov/target/uni-resolver-driver-did-sov-0.3-SNAPSHOT.war /jetty-distribution-9.4.15.v20190215/webapps/ROOT.war

# variables

ENV uniresolver_driver_did_sov_libIndyPath=
ENV uniresolver_driver_did_sov_poolConfigs="_;/opt/driver-did-sov/sovrin/bcovrin-dev.txn;"
ENV uniresolver_driver_did_sov_poolVersions="_;2;"
ENV uniresolver_driver_did_sov_walletName=wallet

# Add user
RUN adduser --disabled-login didsov

WORKDIR jetty-distribution-9.4.15.v20190215/

# Switch to the didsov user (non root)
USER didsov

# Starts jetty
ENTRYPOINT ["java", "-jar", "start.jar", "-Dslf4j=true", "-Dlog4j2.configurationFile=/opt/driver-did-sov/src/test/resources/log4j2-test.properties"]
