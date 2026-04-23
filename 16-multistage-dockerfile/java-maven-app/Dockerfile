FROM maven:3.9

WORKDIR /app
COPY . .
RUN mvn clean package

CMD ["java", "-jar", "target/app.jar"]