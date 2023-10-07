REGISTRY=funcx27
ARTHAS_VERSION=3.7.1
MAVEN_VERSION=3.9.5
MVND_VERSION=1.0-m7

JDK_VERSIONS:=$(shell echo 8u342-jdk 11.0.16-jdk)
.PHONY: download jdks push

download:
	@mkdir -p arthas mvn
	@if [ ! -f arthas/arthas-packaging-${ARTHAS_VERSION}.deb ];then \
	  wget https://maven.aliyun.com/repository/public/com/taobao/arthas/arthas-packaging/${ARTHAS_VERSION}/arthas-packaging-${ARTHAS_VERSION}.deb -O arthas/arthas-packaging-${ARTHAS_VERSION}.deb;\
	fi

	@if [ ! -f mvn/apache-maven-${MAVEN_VERSION}-bin.tar.gz ];then \
	  wget --no-check-certificate https://dlcdn.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz -O mvn/apache-maven-${MAVEN_VERSION}-bin.tar.gz;\
	fi
	@if [ ! -f mvn/maven-mvnd-${MVND_VERSION}-linux-amd64.tar.gz ];then \
	  wget --no-check-certificate https://downloads.apache.org/maven/mvnd/${MVND_VERSION}/maven-mvnd-${MVND_VERSION}-m39-linux-amd64.tar.gz -O mvn/maven-mvnd-${MVND_VERSION}-linux-amd64.tar.gz;\
	fi
$(JDK_VERSIONS):
	$(eval VERSION:=$@)
	sed  -e  '/arthas/d' -e '/ARTHAS/d' Dockerfile | docker build  --build-arg JDK_VERSION=${VERSION} -t  ${REGISTRY}/openjdk:${VERSION}  -
	docker build --build-arg JDK_VERSION=${VERSION} --build-arg ARTHAS_VERSION=${ARTHAS_VERSION}  -t ${REGISTRY}/openjdk:${VERSION}-arthas -f Dockerfile arthas
	sed  -e  '/arthas/d' -e '/ARTHAS/d' Dockerfile Dockerfile-mvn > mvn/Dockerfile
	docker build --build-arg JDK_VERSION=${VERSION} --build-arg MAVEN_VERSION=${MAVEN_VERSION} -t  ${REGISTRY}/openjdk:${VERSION}-mvn mvn
	@echo  ${REGISTRY}/openjdk:${VERSION} >> /tmp/jdk_images.txt
	@echo  ${REGISTRY}/openjdk:${VERSION}-arthas >> /tmp/jdk_images.txt
	@echo  ${REGISTRY}/openjdk:${VERSION}-mvn >> /tmp/jdk_images.txt
	@sort -u /tmp/jdk_images.txt > jdk_images.txt
jdks:  $(JDK_VERSIONS)

push:
	cat jdk_images.txt | xargs -i docker push {}
