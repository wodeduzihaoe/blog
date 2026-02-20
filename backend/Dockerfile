FROM eclipse-temurin:17-jdk-alpine

WORKDIR /app

# Copy Maven wrapper and pom.xml first for better caching
COPY pom.xml .
COPY .mvn .mvn
COPY mvnw .

# Copy source code
COPY src ./src

# Build the application
RUN ./mvnw clean package -DskipTests

# Run the application
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "target/blog-backend-1.0.0.jar"]

