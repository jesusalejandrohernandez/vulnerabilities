FROM hashicorp/vault:1.17 as vault

FROM eclipse-temurin:22.0.2_9-jre-alpine

RUN addgroup -S appuser && adduser -S appuser -G appuser

COPY --from=vault --chown=appuser /bin/vault /bin/vault

RUN mkdir /vault \
    && chown appuser /vault \
    && chmod 700 /vault \
    && mkdir /deployments \
    && chown appuser /deployments \
    && chmod 750 /deployments \
    && mkdir /config \
    && chown appuser /config \
    && chmod 750 /config \
    && apk upgrade --no-cache \
    && apk add --no-cache openssl \
    && chown appuser /opt/java/openjdk/lib/security/cacerts \
    && chmod 600 /opt/java/openjdk/lib/security/cacerts

EXPOSE 8081
EXPOSE 9779

USER appuser

ENV JAVA_OPTS="-XX:+UseContainerSupport -Djava.security.egd=file:/dev/./urandom -Djavax.net.ssl.trustStore=${KEY_PATH} -Djavax.net.ssl.trustStorePassword=${KEY_PASSWORD} -Djavax.net.ssl.keyStore=${KEY_PATH} -Djavax.net.ssl.keyStorePassword=${KEY_PASSWORD} -Dintegracion.kafka.topic=${KAFKA_TOPIC} -Dintegracion.kafka.brokers=${KAFKA_BROKERS} -Dintegracion.kafka.sslEndpointAlgorithm='' -Dintegracion.kafka.securityProtocol=${KAFKA_SECURITY_PROTOCOL} -Dintegracion.kafka.sslTruststorePassword=${KEY_PASSWORD}"