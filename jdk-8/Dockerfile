FROM openjdk:8u121-jdk

ENV SBT_VERSION 0.13.11
ENV MVN_VERSION 3.5.2

WORKDIR /tmp

RUN \
  curl -L -o sbt-$SBT_VERSION.deb https://dl.bintray.com/sbt/debian/sbt-$SBT_VERSION.deb && \
  dpkg -i sbt-$SBT_VERSION.deb && \
  rm sbt-$SBT_VERSION.deb && \
  apt-get update && \
  apt-get install sbt && \
  sbt sbtVersion

RUN \
    mkdir -p /usr/share/maven /usr/share/maven/ref \
    && wget --no-check-certificate --no-cookies https://archive.apache.org/dist/maven/maven-3/${MVN_VERSION}/binaries/apache-maven-${MVN_VERSION}-bin.tar.gz \
    && wget --no-check-certificate --no-cookies https://archive.apache.org/dist/maven/maven-3/${MVN_VERSION}/binaries/apache-maven-${MVN_VERSION}-bin.tar.gz.md5 \
    && echo "$(cat apache-maven-${MVN_VERSION}-bin.tar.gz.md5) apache-maven-${MVN_VERSION}-bin.tar.gz" | md5sum -c \
    && tar -zvxf apache-maven-${MVN_VERSION}-bin.tar.gz -C /opt/ \
    && ln -s /opt/apache-maven-${MVN_VERSION} /opt/maven \
    && rm -f apache-maven-${MVN_VERSION}-bin.tar.gz \
    && rm -f apache-maven-${MVN_VERSION}-bin.tar.gz.md5

RUN update-alternatives --install "/usr/bin/mvn" "mvn" "/opt/maven/bin/mvn" 1 && \
    update-alternatives --set "mvn" "/opt/maven/bin/mvn" 

COPY settings-docker.xml /usr/share/maven/ref/
COPY pom.xml /tmp/pom.xml
RUN mvn -B -f /tmp/pom.xml -s /usr/share/maven/ref/settings-docker.xml dependency:resolve

# change to root folder
WORKDIR /root
