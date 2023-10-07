ARG JDK_VERSION=
FROM openjdk:${JDK_VERSION}
RUN sed -i 's/deb.debian.org/mirror.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list &&\
    apt-get update &&\
    apt-get install -y telnet socat netcat curl iproute2 tcpdump traceroute iputils-tracepath iputils-ping net-tools &&\
    apt-get clean
ARG ARTHAS_VERSION=3.7.1

COPY arthas-packaging-${ARTHAS_VERSION}.deb .
RUN dpkg -i arthas-packaging-${ARTHAS_VERSION}.deb && rm -f arthas-packaging-${ARTHAS_VERSION}.deb &&\
    mkdir -p /root/.arthas/lib/latest &&\
    ln -s /usr/share/arthas/ /root/.arthas/lib/latest/arthas
ENV TZ Asia/Shanghai
