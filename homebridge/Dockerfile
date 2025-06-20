FROM homebridge/homebridge:latest

RUN apt-get update && \
    apt-get install -y samba-common-bin smbclient && \
    apt-get clean && rm -rf /var/lib/apt/lists/*
