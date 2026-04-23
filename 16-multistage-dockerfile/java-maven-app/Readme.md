# 🚀 Multi-Stage Docker for Java (Maven) 

## 📌 Application Overview

This is a simple Java application:

-   Prints: "Hello from Java and this is a Docker Build 🚀"
-   Built using Maven
-   Generates an executable JAR file

------------------------------------------------------------------------

## 📦 Project Files

### `src/main/java/com/example/App.java`

Contains the main method for execution

### pom.xml

Defines:
- Java version (17)
- Build configuration
- JAR packaging setup

------------------------------------------------------------------------

## 🐳 Existing Dockerfile (Typical Approach)

``` dockerfile
FROM maven:3.9

WORKDIR /app
COPY . .

RUN javac src/main/java/com/example/App.java

CMD ["java", "com.example.App"]
```

------------------------------------------------------------------------

## ❌ Issues with Existing Dockerfile

-   Compiles code inside runtime image
-   Includes unnecessary build tools
-   Larger image size
-   Not using Maven (despite pom.xml present)
-   Not production-ready

------------------------------------------------------------------------

## 🤔 Why Multi-Stage?

Java projects usually: 1. Build using Maven 2. Run using only JAR

Multi-stage helps separate: - Build stage (Maven) - Runtime stage (JRE
only)

------------------------------------------------------------------------

## ⚖️ Comparison

| Feature       | Single Stage | Multi-Stage |
|---------------|--------------|-------------|
| Image Size    | Large ❌     | Small ✅     |
| Build Process | Mixed ❌     | Clean ✅     |
| Efficiency    | Low ❌       | High ✅      |

------------------------------------------------------------------------

## ✅ Multi-Stage Dockerfile

``` dockerfile
# Stage 1: Build with Maven
FROM maven:3.9.9-eclipse-temurin-17 AS builder

WORKDIR /app
COPY pom.xml .
COPY src ./src

RUN mvn clean package -DskipTests

# Stage 2: Run with JRE only
FROM eclipse-temurin:17-jre

WORKDIR /app
COPY --from=builder /app/target/*.jar app.jar

CMD ["java", "-jar", "app.jar"]
```

------------------------------------------------------------------------

## � Build Result Comparison

Before and after building the images, the size difference is visible from the Docker image listing:

```text
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
java2        latest    636b3fad7fbb   5 minutes ago    289MB
java         latest    67202746e9f3   2 hours ago      535MB
```

------------------------------------------------------------------------

## �🔍 Explanation

### Stage 1 (Builder)

-   Uses Maven image
-   Builds JAR file
-   Handles dependencies

### Stage 2 (Runtime)

-   Uses lightweight JRE image
-   Copies only final JAR
-   No build tools included

------------------------------------------------------------------------

## 🧠 Key Takeaways

-   Never mix build and runtime
-   Always use Maven for Java projects
-   Use multi-stage for smaller, cleaner images
-   Production images should contain only runtime essentials

------------------------------------------------------------------------

Happy Learning Docker 🐳🚀
