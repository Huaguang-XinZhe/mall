# 构建阶段
FROM maven:3.8-openjdk-8 AS builder
WORKDIR /app
COPY . .
RUN mvn clean package -pl mall-portal -am -DskipTests

# 运行阶段
FROM eclipse-temurin:8-jre-alpine
WORKDIR /app
COPY --from=builder /app/mall-portal/target/*.jar app.jar
EXPOSE 8085
ENTRYPOINT ["java", "-XX:+UseContainerSupport", "-Djava.security.egd=file:/dev/./urandom", "-jar", "app.jar"] 