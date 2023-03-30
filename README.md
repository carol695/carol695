# carol695

## Generales

1. Dependencias Spark

* Spark
* Spark api
* Spark simple binding

## Implementación sin Docker

1. Instalación Java 17

```
sudo yum install java-17-amazon-corretto-devel
```

2. Descomprimir archivos

```
unzip {nombre-archivo}
```

3. Ejecutar clase java en linux

```
java -cp "target/classes:target/dependency/*" org.example.nameClase
```

## Implementación en Docker


1. DockerFile

```
FROM openjdk:8

WORKDIR /usrapp/bin

ENV PORT 6000

COPY /target/classes /usrapp/bin/classes
COPY /target/dependency /usrapp/bin/dependency

CMD ["java","-cp","./classes:./dependency/*","org.example.nameClase"]
```

2. Docker Compose

```
version: '2'

services:
  log1:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: log1
    ports:
      - "34000:6000"
      
  db:
    image: mongo:3.6.1
    container_name: db
    volumes:
      - mongodb:/data/db
      - mongodb_config:/data/configdb
    ports:
      - 27017:27017
    command: mongod

volumes:
  mongodb:
  mongodb_config:
```

3. Construir y Crear los servicios en Docker

```
docker-compose build 
docker-compose up -d 
```

4. Subir imagenes a docker hube

```
docker tag {nombre-imagen-en-docker-local} {usernameDockerHub}/{nombre-repoDockerHub}
docker login
docker push {usernameDockerHub}/{nombre-repoDockerHub}:latest
```

5. Instalación Docker en linux

```
sudo yum update -y
sudo yum install docker
```

6. Correr imagenes de dockerhub en AWS

Inicie el servicio de docker
```
sudo service docker start
```
Configure su usuario en el grupo de docker para no tener que ingresar “sudo” cada vez que invoca un comando
sudo usermod -a -G docker ec2-user

Desconectes de la máquina virtual e ingrese nuevamente para que la configuración de grupos de usuarios tenga efecto.
```
docker run -d -p {puerto}:6000 --name {nombre-aws} {usernameDockerHub}/{nombre-repoDockerHub}
```

7. Verificar contenedores corriendo

```
docker ps
```

Abra los puertos de entrada del security group de la máxima virtual para acceder al servicio

Verifique que pueda acceder  en una url similar a esta (la url específica depende de los valores de su maquina virtual EC2)
http://ec2-35-175-205-168.compute-1.amazonaws.com:42002/hello
### SPARK WEB 

```
package org.example;

import static spark.Spark.*;

public class SparkWeb {


    public static void main(String[] args) {
        port(getPort());
        staticFileLocation("/public");
        get("/hello", (req,res) -> "Hello Docker!");
    }


    private static String isPalindrome(){

        return null;
    }

    private static String isJson(){
        return null;
    }
    private static int getPort() {
        if (System.getenv("PORT") != null) {
            return Integer.parseInt(System.getenv("PORT"));
        }
        return 4567;
    }

}

```

### POM
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>AREP-TALLER7</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <!-- https://mvnrepository.com/artifact/com.sparkjava/spark-core -->
        <dependency>
            <groupId>com.sparkjava</groupId>
            <artifactId>spark-core</artifactId>
            <version>2.9.4</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-api -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>2.0.5</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-simple -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>2.0.6</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-api -->

        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.8.6</version>
        </dependency>


    </dependencies>


    <!-- build configuration -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <version>3.0.1</version>
                <executions>
                    <execution>
                        <id>copy-dependencies</id>
                        <phase>package</phase>
                        <goals><goal>copy-dependencies</goal></goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

</project>
```

