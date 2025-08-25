FROM eclipse-temurin:21.0.8_9-jdk-jammy
ADD target/*.jar service.jar
ENTRYPOINT ["java", "-jar", "/service.jar"]