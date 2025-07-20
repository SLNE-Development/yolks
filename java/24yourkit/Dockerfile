FROM        --platform=$TARGETOS/$TARGETARCH eclipse-temurin:22-jdk-noble

LABEL       author="Michael Parker" maintainer="parker@pterodactyl.io"

LABEL       org.opencontainers.image.source="https://github.com/pterodactyl/yolks"
LABEL       org.opencontainers.image.licenses=MIT

RUN         apt update -y \
	&& apt install -y \
	curl \
	lsof \
	ca-certificates \
	openssl \
	git \
	tar \
	sqlite3 \
	fontconfig \
	tzdata \
	iproute2 \
	libfreetype6 \
	tini \
	zip \
	unzip

RUN wget https://www.yourkit.com/download/docker/YourKit-JavaProfiler-2025.3-docker.zip -P /tmp/ && \
	unzip /tmp/YourKit-JavaProfiler-2025.3-docker.zip -d /usr/local && \
	rm /tmp/YourKit-JavaProfiler-2025.3-docker.zip

## Setup user and working directory
RUN         useradd -m -d /home/container -s /bin/bash container
USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

STOPSIGNAL SIGINT

COPY        --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh
ENTRYPOINT    ["/usr/bin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]
