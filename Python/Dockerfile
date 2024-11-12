FROM python:3.12.5-slim-bookworm

LABEL source-image=python:3.12.5-slim-bookworm repository=evc/stable

ENV TZ=America/Bogota

RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get clean && \
    apt-get autoclean && \
    apt-get autoremove -y && \
    apt-get check && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir -p /home/appuser && \
    HOME=/home/appuser && \
    chmod -R 0755 /home/appuser

RUN addgroup --system --gid 10101 appuser && \
    adduser --system --disabled-password --shell /sbin/nologin --home /home/appuser --ingroup appuser appuser

USER appuser

WORKDIR /home/appuser