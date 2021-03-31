FROM universalresolver/driver-did-sov as maven

# build driver-did-sov

ADD . /opt/driver-did-sov
RUN cd /opt/driver-did-sov && mvn clean install package -N -DskipTests dependency:resolve dependency:resolve-plugins -P build-war

# Add user
RUN adduser -D didsov

# Download jetty
RUN wget https://repo1.maven.org/maven2/org/eclipse/jetty/jetty-distribution/9.4.15.v20190215/jetty-distribution-9.4.15.v20190215.tar.gz
RUN tar xvf jetty-distribution-9.4.15.v20190215.tar.gz

# Copy build artifact (.war file) into jetty from 'maven' stage.
COPY /opt/driver-did-sov/target/uni-resolver-driver-did-sov-0.3-SNAPSHOT.war jetty-distribution-9.4.15.v20190215/webapps/ROOT.war

WORKDIR jetty-distribution-9.4.15.v20190215/

# Switch to the obp user (non root)
USER didsov

ENV uniresolver_driver_did_sov_libIndyPath=
ENV uniresolver_driver_did_sov_poolConfigs=_;/opt/driver-did-sov/bcovrin-dev.txn
ENV uniresolver_driver_did_sov_poolVersions=_;2
ENV uniresolver_driver_did_sov_walletName=wallet

# Starts jetty
ENTRYPOINT ["java", "-jar", "start.jar"]
