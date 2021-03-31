# Dockerfile for universalresolver/driver-did-sov

FROM universalresolver/driver-did-sov

RUN apt-get -y update && \
    apt-get install -y --no-install-recommends wget && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Download jetty
RUN wget https://repo1.maven.org/maven2/org/eclipse/jetty/jetty-distribution/9.4.15.v20190215/jetty-distribution-9.4.15.v20190215.tar.gz
RUN tar xvf jetty-distribution-9.4.15.v20190215.tar.gz

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
