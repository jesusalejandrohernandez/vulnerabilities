FROM python:3.12.5-slim-bookworm

LABEL source-image=python:3.12.5-slim-bookworm repository=evc/stable

ENV TZ=America/Bogota

# Actualización del sistema operativo base y limpieza de paquetes innecesarios
RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get clean && \
    apt-get autoclean && \
    apt-get autoremove -y && \
    apt-get check && \
    rm -rf /var/lib/apt/lists/*

# Creación de un usuario no root
RUN mkdir -p /home/appuser && \
    addgroup --system --gid 10101 appuser && \
    adduser --system --disabled-password --shell /sbin/nologin --home /home/appuser --ingroup appuser appuser

# Establecer el directorio de trabajo y el usuario
WORKDIR /home/appuser
USER appuser

# Exponer solo los puertos necesarios (ajustar según la aplicación)
# EXPOSE 8000

# Implementar herramientas de análisis estático de código (SAST) y logging
# (Este paso se debe realizar en el entorno de desarrollo y no se incluye en el Dockerfile)