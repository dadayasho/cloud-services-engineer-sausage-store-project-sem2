FROM maven:3.8.2-openjdk-16-slim AS builder
ARG VERSION
WORKDIR /usr/src/app
COPY pom.xml ./
COPY src ./src
RUN mvn package -Dversion.application=${VERSION}

FROM eclipse-temurin:21-jdk-alpine
WORKDIR /app
RUN addgroup -S jaruser \
    && adduser -S -D -H -G jaruser -s /sbin/nologin jaruser \
    && apk add --no-cache dumb-init curl \
    && mkdir -p /logs/reports \
    && chown -R jaruser:jaruser /logs

COPY --chown=jaruser:jaruser --from=builder /usr/src/app/target/*.jar ./sausage-store.jar

EXPOSE 8080
USER jaruser
ENTRYPOINT ["dumb-init", "java", "-jar", "-Dmyserver.bindPort=8080", "./sausage-store.jar"]