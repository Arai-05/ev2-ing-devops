FROM maven:3.9-eclipse-temurin-26 AS build
WORKDIR /app

# Se copia primero el pom para aprovechar la cache de capas de dependencias
COPY pom.xml .
RUN mvn -B -q dependency:go-offline

COPY src ./src
#Las pruebas ya se ejecutan en el pipeline (job test)
RUN mvn -B -q clean package -DskipTests


FROM eclipse-temurin:21-jre-alpine
WORKDIR /app

RUN addgroup -S app && adduser -S app -G app

COPY --from=build /app/target/*.jar app.jar

USER app
EXPOSE 9004

ENTRYPOINT ["java", "-XX:MaxRAMPercentage=75", "-jar", "app.jar"]
