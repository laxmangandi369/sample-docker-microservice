FROM alpine:latest AS build
ENV JAVA_HOME /opt/jdk/jdk-18
ENV PATH $JAVA_HOME/bin:$PATH

ADD https://download.java.net/java/early_access/alpine/11/binaries/openjdk-18-ea+11_linux-x64-musl_bin.tar.gz /opt/jdk/
RUN tar -xzvf /opt/jdk/openjdk-18-ea+11_linux-x64-musl_bin.tar.gz -C /opt/jdk/
RUN ["jlink", "--compress=2", \
     "--module-path", "/opt/jdk/jdk-18/jmods/", \
     "--add-modules", "java.base,java.logging,java.naming,java.xml,java.desktop,jdk.httpserver,jdk.unsupported,java.security.jgss", \
     "--no-header-files", "--no-man-pages", \
     "--output", "/springboot-runtime"]

FROM alpine:latest
COPY --from=build  /springboot-runtime /opt/jdk
ENV PATH=$PATH:/opt/jdk/bin
COPY ../target/spring-microservice-1.0-SNAPSHOT.jar /opt/app/
CMD ["java", "-showversion", "-jar", "/opt/app/spring-microservice-1.0-SNAPSHOT.jar"]