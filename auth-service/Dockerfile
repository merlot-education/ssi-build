FROM maven:3-eclipse-temurin-17-alpine AS build
RUN apk add git

# tests (in this case: including build) have to run in a non-privileged context: https://github.com/zonkyio/embedded-database-spring-test#running-tests-in-docker-does-not-work
RUN addgroup -S -g 1000 test
RUN adduser -D -S -G test -u 1000 -s /bin/ash test
USER test
WORKDIR /home/test

RUN git clone https://gitlab.eclipse.org/eclipse/xfsc/authenticationauthorization.git
RUN mvn -ntp -f /home/test/authenticationauthorization/pom.xml clean package

# based on https://gitlab.eclipse.org/eclipse/xfsc/authenticationauthorization/-/blob/main/service/Dockerfile?ref_type=heads
FROM eclipse-temurin:17-jre-alpine
COPY --from=build /home/test/authenticationauthorization/service/target/auth-service-1.1.0-SNAPSHOT.jar auth-service-1.1.0-SNAPSHOT.jar
ENTRYPOINT ["java", "-jar", "/auth-service-1.1.0-SNAPSHOT.jar"]
