FROM openjdk:11-jre-slim as builder
WORKDIR application
ARG JAR_FILE
ADD ${JAR_FILE} ${JAR_FILE}
RUN java -Djarmode=layertools -jar ${JAR_FILE} extract

FROM openjdk:11-jre-slim

EXPOSE 8080

WORKDIR application
#CMD echo Hello World application
COPY --from=builder /application/dependencies/ ./
COPY --from=builder /application/spring-boot-loader/ ./
COPY --from=builder /application/snapshot-dependencies/ ./
COPY --from=builder /application/application/ ./
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom", "org.springframework.boot.loader.JarLauncher"]


# builder
# steps performed:
# 1. Start will the Open JDK Java 11 JRE slim image
# 2. Creates working directory called application
# 3. Copies the spring-boot fat jar to working directory
# 4. Calls the spring-boot layer tools to extract the layer files
#FROM openjdk:11-jre-slim
#VOLUME /tmp
#ARG JAR_FILE
#COPY ${JAR_FILE} app.jar
#ENTRYPOINT ["java", "-jar", "/app.jar"]
