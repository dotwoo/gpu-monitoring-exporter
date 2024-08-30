FROM nvidia/dcgm-exporter:3.3.7-3.5.0-ubuntu22.04

RUN sed -i "s|archive.ubuntu.com|mirrors.bfsu.edu.cn|g" /etc/apt/sources.list
RUN apt-get update \
    && apt-get install -y ca-certificates \
    && apt-get install wget -y \
    && wget -qO- https://get.docker.com | sh \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

COPY nvidia-smi-ps /usr/local/bin
COPY nvidia-smi-exporter /usr/local/bin

RUN chmod +x /usr/local/bin/nvidia-smi-*

ENTRYPOINT [ "nvidia-smi-exporter" ]
