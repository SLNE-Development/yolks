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

RUN mkdir -p /opt/jprofiler && \
	wget https://download.ej-technologies.com/jprofiler/jprofiler_linux_14_0_6.tar.gz -O /tmp/jprofiler.tar.gz && \
	tar -xzf /tmp/jprofiler.tar.gz --strip-components=1 -C /opt/jprofiler && \
	rm /tmp/jprofiler.tar.gz

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
