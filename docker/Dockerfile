FROM universalresolver/driver-did-sov

# build driver-did-sov

ADD . /opt/driver-did-sov
RUN cd /opt/driver-did-sov && mvn clean install package -N -DskipTests dependency:resolve dependency:resolve-plugins -P build-war

# variables

ENV uniresolver_driver_did_sov_libIndyPath=
ENV uniresolver_driver_did_sov_poolConfigs=_;./sovrin/mainnet.txn;staging;./sovrin/stagingnet.txn;builder;./sovrin/buildernet.txn;danube;./sovrin/danube.txn;bcovrin;./sovrin/bcovrin-dev.txn
ENV uniresolver_driver_did_sov_poolVersions=_;2;staging;2;builder;2;danube;2;bcovrin;2
ENV uniresolver_driver_did_sov_walletName=default

# done

EXPOSE 8080

RUN chmod a+rx /opt/driver-did-sov/docker/run-driver-did-sov.sh
RUN chmod a+rwx /root/.m2
CMD "/opt/driver-did-sov/docker/run-driver-did-sov.sh"
