# Multi-stage build for Spring Boot application
FROM eclipse-temurin:17-jdk-alpine AS builder

# Set working directory
WORKDIR /app

# Copy Maven wrapper and pom.xml
COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .

RUN chmod +x ./mvnw
# Download dependencies (this layer will be cached if pom.xml doesn't change)
RUN ./mvnw dependency:go-offline -B


# Copy source code
COPY src src

# Build the application
RUN ./mvnw clean package -DskipTests
RUN mv target/attendance-*.jar target/attendance.jar

# Runtime stage
FROM eclipse-temurin:17-jdk-alpine

# Create app user for security
RUN addgroup -g 1001 -S appgroup && \
    adduser -u 1001 -S appuser -G appgroup

RUN mkdir -p /app/logs && chown -R appuser:appgroup /app/logs   

# Set working directory
WORKDIR /app

# Copy JAR from builder stage
COPY --from=builder /app/target/attendance.jar attendance.jar

# Change ownership to app user
RUN chown appuser:appgroup attendance.jar

# Switch to app user
USER appuser

# Expose port
EXPOSE 8080

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=60s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:8080/actuator/health || exit 1

# Set JVM options for container
ENV JAVA_OPTS="-Xmx512m -Xms256m -Djava.security.egd=file:/dev/./urandom"

# Entry point
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar attendance.jar"]
