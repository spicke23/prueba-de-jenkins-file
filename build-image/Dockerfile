# se especifica la imagen base
FROM ubuntu:latest
# Actualizamos la paqueteria de ubuntu e instalamos el servidor web Nginx (Angular)
RUN apt-get update \
    && apt-get install -y nginx
# Instalar OpenJDK 17, version necesaria para poder ejecutar la aplicacion de java (jar)
RUN apt-get install -y openjdk-17-jdk
# Argumento
ARG JAR_FILE=target/devops-apirest-0.0.1-SNAPSHOT.jar
# Añadir el archivo Jar especificado por la variable ARG
ADD ${JAR_FILE} app.jar
VOLUME /tmp
RUN rm -vfr /usr/share/nginx/html/*
COPY nginx.conf /etc/nginx/nginx.conf
COPY dist/crudangular /usr/share/nginx/html
EXPOSE 80 8080
CMD service nginx start && java -jar app.jar
