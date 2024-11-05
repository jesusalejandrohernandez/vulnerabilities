FROM python:3.12.5-slim-bookworm

LABEL source-image=python:3.12.5-slim-bookworm repository=evc/stable

ENV TZ=America/Bogota

# Crear un usuario no root y establecer el directorio de trabajo
RUN addgroup --system --gid 10101 appuser && \
    adduser --system --disabled-password --shell /sbin/nologin --home /home/appuser --ingroup appuser appuser && \
    mkdir -p /home/appuser && \
    chown -R appuser:appuser /home/appuser

USER appuser

WORKDIR /home/appuser

# Actualizar el sistema operativo base y limpiar
RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get clean && \
    apt-get autoclean && \
    apt-get autoremove -y && \
    apt-get check && \
    rm -rf /var/lib/apt/lists/*

# Exponer solo los puertos necesarios (ajustar según la aplicación)
EXPOSE 8000

# Implementar herramientas de análisis estático de código (SAST) y logging
# (Este paso se sugiere realizarlo en el entorno de desarrollo y no se incluye en el Dockerfile)