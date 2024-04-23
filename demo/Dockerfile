# Use official OpenJDK 11 image as the base image
FROM openjdk:17-jdk-slim AS build

# Set the working directory in the container
WORKDIR /app

# Copy Maven executable to the image
COPY mvnw .
COPY .mvn .mvn

# Copy the pom.xml file
COPY pom.xml .

# Download all Maven dependencies (dependencies will be cached if the pom.xml file hasn't changed)
RUN ./mvnw dependency:go-offline -B

# Copy the project source
COPY src src

# Package the application
RUN ./mvnw package -DskipTests

# Create a new image from the base image with the JAR file
FROM openjdk:17-jre-slim

WORKDIR /app

# Copy the JAR file from the 'build' stage to the working directory in the container
COPY --from=build /app/target/*.jar app.jar

# Specify the command to run the Spring Boot application
CMD ["java", "-jar", "app.jar"]
