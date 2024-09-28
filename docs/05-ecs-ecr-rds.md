# Desplegando Nuestros Proyectos 🚀

Para esta sección, queremos agradecer especialmente al profesor Geraldo Colchado por su apoyo y orientación en la creación de este material 👨‍🏫. Él está a cargo del siguiente curso, Cloud Computing CS2032 🌥️, donde profundizarán y resolverán todas sus dudas.

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

Este comando compilará el proyecto y generará el archivo JAR en la carpeta `target`. Cuando commitiemos el proyecto, asegúrate de que el archivo JAR se haya generado correctamente y esté en la carpeta `target`. 

```
target
├── classes
├── generated-sources
├── generated-test-sources
├── maven-archiver
├── maven-status
├── surefire-reports
├── test-classes
├── hackathon-0.0.1-SNAPSHOT.jar
└── hackathon-0.0.1-SNAPSHOT.jar.original
```

## Paso 2: Abrir la Cloud Shell de AWS

Para desplegar nuestra aplicación en ECS, necesitamos acceder a la consola de AWS. Podemos hacerlo a través de la Cloud Shell de AWS, que nos permite ejecutar comandos de AWS directamente en el navegador.

Realizaremos dos acciones que no son las mejores prácticas, pero que nos ayudarán a entender el sistema. Primero, ejecutaremos comandos directamente en la Cloud Shell. 

Segundo, pondremos nuestro repositorio en modo público, lo cual no se recomienda por la exposición del código. Sin embargo, esto es solo para la configuración inicial. 

![AWS Cloud Shell](../media/05/1.gif)

Clonamos nuestro repositorio en la Cloud Shell de AWS:

```bash
git clone <url-repo>
```

Ingresamos a la carpeta del proyecto:

```bash
cd <nombre-proyecto>
```

## Paso 3: Crear un Repositorio en ECR 🗂️

Amazon ECR (Elastic Container Registry) es el servicio equivalente de AWS a Docker Hub, permitiéndonos almacenar, administrar y desplegar nuestras imágenes de contenedores de manera eficiente.

1. **Abrir la Consola de AWS**: Navega a la consola de AWS. 🖥️
2. **Buscar ECR**: Usa la barra de búsqueda en la parte superior para encontrar el servicio ECR. 🔍
3. **Crear Repositorio**: Una vez en la página de ECR, sigue las instrucciones para crear un nuevo repositorio y dale el nombre que prefieras. 📛

![ECR AWS CREATION](../media/05/3.gif)

## Paso 4: Construir y Subir la Imagen al Repositorio de ECR 🚢

1. **Autenticar Docker con ECR**: Primero, necesitamos autenticar Docker con nuestro repositorio de ECR. Copiamos el URI del repositorio de ECR que acabamos de crear y ejecutamos el siguiente comando desde la Cloud Shell de AWS:

```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <account_id>.dkr.ecr.us-east-1.amazonaws.com
```

Para extraer nuestro `account_id`, dirígete a la esquina superior derecha al costado de la región y haz clic. Esto abrirá una interfaz donde podemos copiar el ID de la cuenta:

![Cuenta ID](../media/05/4.gif)

Si la autenticación es exitosa, veremos un mensaje de éxito similar a este:

```bash
WARNING! Your password will be stored unencrypted in /home/cloudshell-user/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

2. **Construir la Imagen de Docker**: Ahora, construimos la imagen de Docker para nuestra aplicación de Spring Boot. En la terminal, ejecuta:

```bash
docker build -t spring-boot-app .
```

3. **Etiquetar la Imagen**: Necesitamos etiquetar nuestra imagen de Docker con el URI de nuestro repositorio de ECR. 

```bash
docker tag spring-boot-app:latest <account_id>.dkr.ecr.us-east-1.amazonaws.com/spring-boot-app:latest
```

4. **Subir la Imagen a ECR**: Finalmente, subimos la imagen etiquetada a nuestro repositorio de ECR:

```bash
docker push <account_id>.dkr.ecr.us-east-1.amazonaws.com/spring-boot-app:latest
```

![ECR AWS PUSH](../media/05/5.gif)

¡Y listo! Nuestra imagen de Docker se ha subido al repositorio de ECR. Podemos verificarlo en la consola de AWS o directamente en la consola de ECR. 🎉

## Paso 5: Crear Grupos de Seguridad para ECS y RDS 🔒

Para permitir que ECS se comunique con RDS, necesitamos crear un grupo de seguridad para cada uno. Sigamos estos pasos:

1. **Crear Grupo de Seguridad para ECS (sg-ecs)**:
   - **Nombre del Grupo**: sg-ecs
   - **Puertos**: Abrir el puerto 8080 para Spring Boot
   - **Fuente**: Todos los orígenes (0.0.0.0/0)
   - **Egreso**: Permitir todo el tráfico (0.0.0.0/0)

   Esto permitirá que nuestra aplicación Spring Boot reciba tráfico en el puerto 8080.

2. **Crear Grupo de Seguridad para RDS (sg-rds)**:
   - **Nombre del Grupo**: sg-rds
   - **Puertos**: Abrir el puerto 5432 para Postgres
   - **Fuente**: Limitar la entrada únicamente desde el grupo de seguridad sg-ecs
   - **Salida**: Sin reglas de salida (ya que RDS no necesita enviar tráfico saliente)

   Esto asegurará que nuestra base de datos Postgres solo acepte conexiones desde nuestra aplicación ECS.

¡Ahora nuestros servicios podrán comunicarse de forma segura y eficiente! 🛡️🚀

## Paso 6: Crear la Base de Datos en RDS 📊

Para almacenar nuestros datos, vamos a crear una base de datos en Amazon RDS usando el motor PostgreSQL. Sigamos estos pasos para asegurarnos de tener todo correctamente configurado:

1. **Buscar el Servicio RDS**:
   - En la barra de búsqueda, escribimos "RDS" y seleccionamos el servicio Amazon RDS. 🔍

2. **Crear una Nueva Instancia de Base de Datos**:
   - Hacemos clic en "Crear base de datos" y seleccionamos el motor **PostgreSQL**. 🛢️
3. **Configuración Manual**:
   - Seleccionamos la opción de configuración **manual** para tener control total sobre la configuración. ⚙️
   - Establecemos una **contraseña** segura para el usuario administrador (master user). 🔒

4. **Configurar Parámetros Básicos**:
   - Elegimos una clase de instancia y configuramos las opciones de almacenamiento según nuestras necesidades. 📦
   - Por defecto, se creará una URL de la base de datos y una base de datos llamada `postgres`.

5. **Configurar la Conectividad**:
   - Seleccionamos el grupo de seguridad que creamos anteriormente para RDS (`sg-rds`), asegurándonos de que permita conexiones solo desde el grupo de seguridad de ECS (`sg-ecs`). 🔗

6. **Crear y Guardar las Credenciales**:
   - Al finalizar la configuración, copiamos las credenciales de la base de datos (nombre de usuario, contraseña, URL) y las guardamos en un lugar seguro. 🔐
   - Estas credenciales serán necesarias para configurar nuestra aplicación Spring Boot para conectarse a la base de datos.

¡Y eso es todo! Ahora tenemos una base de datos PostgreSQL en Amazon RDS lista para ser utilizada por nuestra aplicación. 🎉

## Paso 7: Crear un Cluster de ECS 🛠️

Para desplegar nuestra aplicación en contenedores, necesitamos crear un cluster en Amazon ECS. Aquí están los pasos detallados:

1. **Buscar ECS**:
   - Usamos la barra de búsqueda de AWS para encontrar el servicio **ECS**. 🔍

2. **Crear Cluster**:
   - Hacemos clic en "Crear cluster" y seleccionamos el tipo de cluster que prefiramos (EC2 o Fargate). 🖥️
   - Para una gestión más sencilla y sin necesidad de administrar servidores, seleccionamos **Fargate**. 🚀

4. **Finalizar y Crear**:
   - Revisamos las configuraciones y hacemos clic en "Crear" para finalizar el proceso. 📝

![ECS AWS CREATION](../media/05/6.gif)

## Paso 8: Definir una tarea de ECS

En este paso, definiremos una tarea de ECS para ejecutar nuestra aplicación Spring Boot en un contenedor. Aquí están los pasos detallados:

1. **Crear una Definición de Tarea**:
   - En la página de ECS, seleccionamos "Tareas" en el menú lateral y hacemos clic en "Crear nueva tarea". 📋
   - Asignamos un nombre a la tarea, seleccionamos el tipo de tarea y configuramos los recursos necesarios (2 vCPU, 4 GB de RAM). 📏
   - Para el rol de la tarea y el rol de ejecución, seleccionamos `LabRole`.

![ECS TASK AWS CREATION](../media/05/8.gif)

2. **Definir el Contenedor**:
   - En la sección de contenedores, asignamos un nombre al contenedor y copiamos el URI de nuestra imagen de Docker en ECR. 🐳
   - Configuramos los puertos de la aplicación, especificando el puerto 8080. 🌐
   - En la sección de límites de recursos, asignamos 2 vCPU y 4 GB de RAM en el límite estricto de memoria y 1 GB en el límite flexible de memoria.
   - Asignamos las variables de entorno necesarias para la base de datos (URL, usuario, contraseña) para que nuestra aplicación Spring Boot pueda conectarse a la base de datos. 🔑

   Cargamos las variables de entorno copiando las credenciales de la base de datos en el contenedor, las cuales se generaron en RDS. Usaremos:
   - `DB_HOST`: la URL de la base de datos.
   - `DB_PORT`: el puerto de la base de datos.
   - `DB_NAME`: el nombre de la base de datos, por defecto `postgres`.
   - `DB_USERNAME`: el nombre de usuario.
   - `DB_PASSWORD`: la contraseña.

   - En volumen de almacenamiento, asignamos un volumen de almacenamiento de 21 GB para almacenar los datos de la aplicación.

3. **Finalizar y Crear**:
   - Revisamos la configuración de la tarea y hacemos clic en "Crear" para finalizar el proceso. 📝

   ![ECS TASK AWS CREATION](../media/05/9.gif)

## Paso 9: Crear un Servicio de ECS

Ahora nos toca crear un servicio de ECS para ejecutar nuestra tarea en el cluster. Aquí están los pasos detallados:

1. **Crear un Servicio**:
   - En la página de ECS, dirigimos a nuestro cluster y hacemos clic en "Crear nuevo servicio". 🚀

2. **Configurar el Servicio**:
   - Asignamos un nombre al servicio y seleccionamos la definición de tarea que creamos anteriormente. 📋
   - En redes, seleccionamos los grupos de subredes y el grupo de seguridad que creamos anteriormente para ECS (`sg-ecs`). 🔒

3. **Crear y Desplegar**:
   - Revisamos la configuración del servicio y hacemos clic en "Crear" para finalizar el proceso. 📝

Demora unos minutos en desplegar el servicio. Una vez completado, podremos ver nuestra aplicación Spring Boot ejecutándose en un contenedor en ECS. 🎉

## Paso 10: Acceder a la Aplicación en ECS

La tarea tiene una dirección IP publica que podemos usar para acceder a nuestra aplicación Spring Boot. 

Nos conectamos a la dirección IP pública de la tarea en el puerto 8080 para acceder a nuestra aplicación. 🌐

¡Y eso es todo! Hemos desplegado nuestra aplicación Spring Boot en un contenedor en Amazon ECS. 🚀

# Actions para Automatizar el Despliegue 🤖

Para automatizar el proceso de despliegue, podemos utilizar GitHub Actions. Aquí hay un ejemplo de un archivo de flujo de trabajo de GitHub Actions para desplegar nuestra aplicación en ECS:

```yaml
name: Java CI with Maven

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Set up JDK 21
        uses: actions/setup-java@v3
        with:
          java-version: '21'
          distribution: 'temurin'
          cache: maven

      - name: Build and test with Maven
        run: mvn -B clean package --file pom.xml

      - name: Build Docker image
        run: |
          docker build -t spring-boot-app .

      - name: Tag Docker image
        env:
          USER_ID: ${{ secrets.USER_ID }}
          ECR_REPOSITORY_NAME: ${{ secrets.ECR_REPOSITORY_NAME }}
        run: |
          docker tag spring-boot-app ${{ secrets.USER_ID }}.dkr.ecr.us-east-1.amazonaws.com/${{ secrets.ECR_REPOSITORY_NAME }}:latest

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-session-token: ${{ secrets.AWS_SESSION_TOKEN }}
          aws-region: ${{ secrets.AWS_REGION }}

      - name: Log in to Amazon ECR
        env:
          AWS_REGION: ${{ secrets.AWS_REGION }}
          USER_ID: ${{ secrets.USER_ID }}
        run: |
          aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $USER_ID.dkr.ecr.$AWS_REGION.amazonaws.com

      - name: Push Docker image to ECR
        env:
          USER_ID: ${{ secrets.USER_ID }}
          ECR_REPOSITORY_NAME: ${{ secrets.ECR_REPOSITORY_NAME }}
        run: |
          docker push $USER_ID.dkr.ecr.us-east-1.amazonaws.com/$ECR_REPOSITORY_NAME:latest

      - name: Update ECS service
        env:
          AWS_REGION: ${{ secrets.AWS_REGION }}
          CLUSTER_NAME: ${{ secrets.CLUSTER_NAME }}
          SERVICE_NAME: ${{ secrets.SERVICE_NAME }}
        run: |
          aws ecs update-service \
            --cluster $CLUSTER_NAME \
            --service $SERVICE_NAME \
            --force-new-deployment \
            --region $AWS_REGION
```

Cargamos en github secrets las variables de entorno necesarias para el despliegue.

¡Y eso es todo! Con este archivo de flujo de trabajo de GitHub Actions, podemos automatizar el proceso de despliegue de nuestra aplicación Spring Boot en Amazon ECS. 🤖🚀
