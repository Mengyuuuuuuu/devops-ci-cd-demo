# ---- Build stage ----
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /workspace

COPY app/pom.xml app/mvnw app/mvnw.cmd /workspace/app/
COPY app/.mvn /workspace/app/.mvn

WORKDIR /workspace/app
RUN ./mvnw -B -DskipTests dependency:go-offline

COPY app/src /workspace/app/src
RUN ./mvnw -B package -DskipTests

# ---- Run stage ----
FROM eclipse-temurin:17-jre
WORKDIR /app

RUN useradd -m appuser
USER appuser

COPY --from=build /workspace/app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","/app/app.jar"]