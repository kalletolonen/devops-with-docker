# Let's install java 8
FROM openjdk:8-jdk-alpine

WORKDIR /app

# Copy the Maven Wrapper and the project files
#COPY mvnw .
#COPY .mvn .mvn
#COPY pom.xml .
#COPY src src

# Build the project with Maven
RUN ./mvnw package

# Make port 8080 available to the world outside this container
EXPOSE 8080

# Run the application when the container launches
CMD ["java", "-jar", "./target/docker-example-1.1.3.jar"]