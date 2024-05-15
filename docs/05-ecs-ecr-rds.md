# Desplegando Nuestros Proyectos 🚀

Hemos utilizado Docker EC2 y DockerHub para desplegar nuestros proyectos. Sin embargo, ahora vamos a migrar a ECS y ECR para esta tarea. ECS (Elastic Container Service) y ECR (Elastic Container Registry) son servicios de AWS que nos permiten desplegar nuestros contenedores de manera más sencilla y escalable, eliminando las limitaciones horarias de AWS Academy.

## Paso 1: Añadir Dockerfile al Proyecto de Spring Boot con Java 21

Primero, crearemos un archivo `Dockerfile` para nuestro proyecto de Spring Boot:

```Dockerfile
FROM openjdk:21-jdk
WORKDIR /app
COPY target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### Eliminación de la Función `contextLoads()`

Eliminamos la función `void contextLoads()` en la clase principal de pruebas. Esta función es una prueba generada automáticamente que verifica si el contexto de la aplicación se carga correctamente. Esta prueba no es necesaria para la aplicación y puede causar problemas en un entorno de contenedor. Ya que trata de levantar el contexto de la aplicación, esta prueba fallará en un contenedor debido a la falta de un entorno de ejecución completo. Por lo tanto, eliminamos esta prueba para evitar problemas en el despliegue.

### Construcción del Proyecto

A continuación, necesitamos construir el proyecto de Spring Boot para generar el archivo JAR. Ejecutamos el siguiente comando en la terminal de IntelliJ IDEA o en la terminal de tu sistema operativo:

```bash
mvn clean package
```

Este comando compilará el proyecto y generará el archivo JAR en la carpeta `target`. Cuando commitiemos el proyecto, asegúrate de que el archivo JAR se haya generado correctamente y esté en la carpeta `target`. Target esta en el .gitignore, por lo que no se subira al repositorio. Para agregarlo al repositorio, usando git add -f target/ para forzar la subida del archivo. 

```bash
git add -f target/
```

Para despues eliminarlo del repositorio, usamos el siguiente comando:

```bash
git rm --cached target/
```

## Paso 2: Abrir la Cloud Shell de AWS

Para desplegar nuestra aplicación en ECS, necesitamos acceder a la consola de AWS. Podemos hacerlo a través de la Cloud Shell de AWS, que nos permite ejecutar comandos de AWS directamente en el navegador. 

Vamos a hacer dos malas práctica y ejecutar comandos directamente en la Cloud Shell. **No es recomendable hacer esto en un entorno de producción**. En un entorno de producción, se recomienda utilizar un sistema de control de versiones y automatización de despliegues para garantizar la consistencia y la trazabilidad de los cambios.  La otra poner nuestro repo en publico, lo cual no es recomendable. 

Clonamos nuestro repositorio en la Cloud Shell de AWS:

```bash
git clone <url-repo>
```

Ingresamos a la carpeta del proyecto:

```bash
cd <nombre-proyecto>
```


## Paso 3: Crear un Repositorio en ECR

Ingresamos a la consola de AWS y buscamos el servicio ECR. Creamos un repositorio con el nombre que deseemos.

1. **Abrir la Consola de AWS**: Navega a la consola de AWS.
2. **Buscar ECR**: Usa la barra de búsqueda para encontrar el servicio ECR.
3. **Crear Repositorio**: Sigue las instrucciones para crear un nuevo repositorio y dale el nombre que prefieras.

## Paso 4: Construir y Subir la Imagen al Repositorio de ECR

1. Autenticar Docker con ECR. Copiamos el URI del repositorio de ECR que acabamos de crear.

```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <account_id>.dkr.ecr.us-east-1.amazonaws.com
```

Nos aparecerá un mensaje de éxito si la autenticación fue exitosa. Con un mensaje como el siguiente:

```bash
WARNING! Your password will be stored unencrypted in /home/cloudshell-user/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

A continuación, construimos la imagen de Docker y la subimos al repositorio de ECR:

```bash
docker build -t spring-boot-app .
```

Para extraer nuestro account_id, nos dirigimos a la esquina superior derecha al cosado de la región y le damos click nos abrira un una interfaz donde copiamos el ID de la cuenta.

```bash
docker tag spring-boot-app:latest <account_id>.dkr.ecr.us-east-1.amazonaws.com/spring-boot-app:latest
```

```bash
docker push <account_id>.dkr.ecr.us-east-1.amazonaws.com/spring-boot-app:latest
```

Ahora, nuestra imagen de Docker se ha subido al repositorio de ECR. Podemos verificarlo en la consola de AWS. Igualmente, podemos ver la imagen en la consola de ECR. Sino ir a el ECR y ver la imagen que acabamos de subir.

## Paso 5: Crear grupos de seguridad para ECS y RDS

Para permitir que ECS se comunique con RDS, necesitamos crear un grupo de seguridad para cada uno.

Otro Grupo de Seguridad para ECS (sg-ecs) puerto 8080 para Spring Boot fuente todos y el egreso a todos.
Crear Grupo de Seguridad para RDS (sg-rds) puerto 5432 para Postgres con fuente sg-ecs entrada y sin salida porque es un rds y no necesita salida.

## Paso 6: Crear la base de datos en RDS

Ingresamos a la consola de AWS y buscamos el servicio RDS. Creamos una nueva instancia de base de datos con el motor PostgreSQL.

Copiamos las credenciales de la base de datos y las guardamos en un lugar seguro. Necesitaremos estas credenciales para configurar nuestra aplicación de Spring Boot.

la ponemos en publica para conectarnos y crear la base de datos. llamaos springbootdb.

```bash
psql -h <rds-endpoint> -p 5432 -U <your-username> -W
```

```sql
CREATE DATABASE springbootdb;
```


## Paso 7: Crear un Cluster de ECS

1. **Buscar ECS**: Usamos la barra de búsqueda para encontrar el servicio ECS.
2. **Crear Cluster**: Creamos un nuevo cluster de ECS y seleccionamos el tipo de cluster que prefiramos.
3. **Configurar Cluster**: Configuramos el cluster con la información necesaria y creamos el cluster.
    - Cluster Name: Nombre del cluster. 
    - EC2 Instance Type: Tipo de instancia de EC2 que se utilizará para el cluster.
    - Key Pair: Par de claves para acceder a las instancias de EC2.
    - Fargate 

## Paso 8: Definir una tarea de ECS

LabRole para rol de tarea y rol de ejecución.

tamaño de la tarea 2 vcpu y 4 gb de ram.

copaimos las credenciales de la base de datos en el contenedor.

## Paso 9: Crear un Servicio de ECS

