# Download datadog java agent
# FROM curlimages/curl as curl
# RUN curl -L https://dtdg.co/latest-java-tracer --output /tmp/dd-java-agent.jar

# Build the jar
FROM openjdk:19-jdk as build
WORKDIR /workspace/app

COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .
COPY src src

RUN ./mvnw package -DskipTests
RUN mkdir -p target/dependency && (cd target/dependency; jar -xf ../*.jar)

# Build the app container
FROM eclipse-temurin:17-jre
WORKDIR /app

# COPY --from=curl /tmp/dd-java-agent.jar dd-java-agent.jar
COPY --from=build /workspace/app/target/helloworld-0.0.1-SNAPSHOT.jar app.jar

ENV DD_SERVICE=myapp
ENV DD_ENV=dev
ENV DD_VERSION=v1
ENV PORT=8080

# COPY opentelemetry-javaagent-1.21.0.jar opentelemetry-javaagent-1.21.0.jar
CMD ["java", "-jar", "/app/app.jar"]
