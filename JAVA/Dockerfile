FROM hashicorp/vault:1.15.2 as vault
FROM eclipse-temurin:17.0.8_7-jre-alpine

USER root

RUN addgroup -S appuser && adduser -S appuser -G appuser

COPY --from=vault --chown=appuser /bin/vault /bin/vault
# Set up permissions for user `appuser`
RUN mkdir /vault \
&& chown appuser /vault \
&& chmod 777 /vault \
# && chmod -R 777 /app \
&& mkdir /deployments \
&& chown appuser /deployments \
&& chmod g+rx /deployments \
&& chown appuser:root /deployments \
# && chown appuser /app/keys-configuration.sh \
# && chmod 777 /app/keys-configuration.sh \
&& mkdir /config \
&& chown appuser /config \
&& chmod g+rwX /config \
&& chown appuser:root /config \
&& apk upgrade --update-cache --available \
&& apk add openssl \
&& rm -rf /var/cache/apk/* \
&& chown appuser /opt/java/openjdk/lib/security/cacerts \
&& chmod 777  /opt/java/openjdk/lib/security/cacerts

RUN UID=1000765000 && \
  apk add shadow && \
  groupadd -g $UID appuser1 && \
  useradd -u $UID -s /sbin/nologin -d /home/appuser -g appuser1 appuser1

# COPY --chown=appuser target/* /deployments/
# COPY --chown=appuser jmx_prometheus_javaagent-0.17.2.jar /deployments/
# COPY --chown=appuser prometheus-config.yml /deployments/

EXPOSE 8081

EXPOSE 9779

USER appuser

ENV JAVA_OPTS="-XX:+UseContainerSupport -Djava.security.egd=file:/dev/./urandom -Djavax.net.ssl.trustStore=${KEY_PATH} -Djavax.net.ssl.trustStorePassword=${KEY_PASSWORD} -Djavax.net.ssl.keyStore=${KEY_PATH} -Djavax.net.ssl.keyStorePassword=${KEY_PASSWORD} -Dintegracion.kafka.topic=${KAFKA_TOPIC} -Dintegracion.kafka.brokers=${KAFKA_BROKERS} -Dintegracion.kafka.sslEndpointAlgorithm='' -Dintegracion.kafka.securityProtocol=${KAFKA_SECURITY_PROTOCOL} -Dintegracion.kafka.sslTruststorePassword=${KEY_PASSWORD}"

# ENTRYPOINT [ "sh", "-c", "vault agent -config=/vault/config/agent-config.hcl && /app/keys-configuration.sh  && java $JAVA_OPTS -javaagent:/deployments/jmx_prometheus_javaagent-0.17.2.jar=9779:/deployments/prometheus-config.yml -jar /deployments/${ARTIFACT_NAME}"]
