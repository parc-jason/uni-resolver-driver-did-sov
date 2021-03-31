FROM universalresolver/driver-did-sov as DID

# build driver-did-sov

ADD . /opt/driver-did-sov
RUN cd /opt/driver-did-sov && mvn clean install package -N -DskipTests dependency:resolve dependency:resolve-plugins -P build-war

FROM jetty as JETTY

COPY --from=DID /opt/driver-did-sov/target/uni-resolver-driver-did-sov-0.3-SNAPSHOT.war /var/lib/jetty/webapps/ROOT.war

ENV uniresolver_driver_did_sov_libIndyPath=
ENV uniresolver_driver_did_sov_poolConfigs=_;./sovrin/bcovrin-dev.txn
ENV uniresolver_driver_did_sov_poolVersions=_;2
ENV uniresolver_driver_did_sov_walletName=wallet
