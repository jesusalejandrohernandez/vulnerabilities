FROM hashicorp/vault:1.17 as vault

FROM eclipse-temurin:22.0.2_9-jre-alpine

# Crear un usuario no root
RUN addgroup -S appuser && adduser -S appuser -G appuser

# Copiar el binario de Vault y establecer permisos
COPY --from=vault --chown=appuser /bin/vault /bin/vault

# Crear directorios necesarios y establecer permisos
RUN mkdir /vault \
    && chown appuser /vault \
    && chmod 700 /vault \
    && mkdir /deployments \
    && chown appuser /deployments \
    && chmod 750 /deployments \
    && mkdir /config \
    && chown appuser /config \
    && chmod 750 /config \
    && apk upgrade --update-cache --available \
    && apk add --no-cache openssl \
    && rm -rf /var/cache/apk/*

# Exponer solo los puertos necesarios
EXPOSE 8081
EXPOSE 9779

# Cambiar al usuario no root
USER appuser

# Establecer variables de entorno necesarias
ENV JAVA_OPTS="-XX:+UseContainerSupport -Djava.security.egd=file:/dev/./urandom -Djavax.net.ssl.trustStore=${KEY_PATH} -Djavax.net.ssl.trustStorePassword=${KEY_PASSWORD} -Djavax.net.ssl.keyStore=${KEY_PATH} -Djavax.net.ssl.keyStorePassword=${KEY_PASSWORD} -Dintegracion.kafka.topic=${KAFKA_TOPIC} -Dintegracion.kafka.brokers=${KAFKA_BROKERS} -Dintegracion.kafka.sslEndpointAlgorithm='' -Dintegracion.kafka.securityProtocol=${KAFKA_SECURITY_PROTOCOL} -Dintegracion.kafka.sslTruststorePassword=${KEY_PASSWORD}"