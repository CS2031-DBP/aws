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

Este comando compilará el proyecto y generará el archivo JAR en la carpeta `target`. Cuando commitiemos el proyecto, asegúrate de que el archivo JAR se haya generado correctamente y esté en la carpeta `target`. Target está en el `.gitignore`, por lo que no se subirá al repositorio. Para agregarlo al repositorio, usamos `git add -f target/` para forzar la subida del archivo:

```bash
git add -f target/
```

Para eliminarlo después del repositorio, usamos el siguiente comando:

```bash
git rm --cached target/
```

## Paso 2: Abrir la Cloud Shell de AWS

Para desplegar nuestra aplicación en ECS, necesitamos acceder a la consola de AWS. Podemos hacerlo a través de la Cloud Shell de AWS, que nos permite ejecutar comandos de AWS directamente en el navegador.

Vamos a hacer dos malas prácticas y ejecutar comandos directamente en la Cloud Shell. **No es recomendable hacer esto en un entorno de producción**. En un entorno de producción, se recomienda utilizar un sistema de control de versiones y automatización de despliegues para garantizar la consistencia y la trazabilidad de los cambios. La otra es poner nuestro repositorio en público, lo cual no es recomendable.

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

Vamos a crear un repositorio en ECR en unos simples pasos. ¡Sigue conmigo!

1. **Abrir la Consola de AWS**: Navega a la consola de AWS. 🖥️
2. **Buscar ECR**: Usa la barra de búsqueda en la parte superior para encontrar el servicio ECR. 🔍
3. **Crear Repositorio**: Una vez en la página de ECR, sigue las instrucciones para crear un nuevo repositorio y dale el nombre que prefieras. 📛

![ECR AWS CREATION](../media/05/3.gif)

Estos pasos te permitirán tener un repositorio listo para recibir tus imágenes de Docker. ¡Adelante, estás haciendo un gran trabajo! 🚀

## Paso 4: Construir y Subir la Imagen al Repositorio de ECR 🚢

¡Es hora de poner en marcha nuestra aplicación! Vamos a construir la imagen de Docker y subirla a nuestro repositorio en ECR.



1. **Autenticar Docker con ECR**: Primero, necesitamos autenticar Docker con nuestro repositorio de ECR. Copiamos el URI del repositorio de ECR que acabamos de crear y ejecutamos el siguiente comando:

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

¡Y listo! Nuestra imagen de Docker se ha subido al repositorio de ECR. Podemos verificarlo en la consola de AWS o directamente en la consola de ECR. 🎉

Sigue estos pasos y tendrás tu aplicación lista para ser desplegada en ECS. ¡Buen trabajo! 🚀

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

3. **Configurar Cluster**:
   - Configuramos el cluster con la información necesaria. Aquí algunos parámetros importantes a configurar:
     - **Cluster Name**: Especificamos el nombre del cluster.
     - **EC2 Instance Type**: (Si seleccionamos EC2) Elegimos el tipo de instancia de EC2 que se utilizará para el cluster. 💻
     - **Key Pair**: (Si seleccionamos EC2) Seleccionamos el par de claves para acceder a las instancias de EC2. 🔑
     - **Fargate**: Si seleccionamos Fargate, no necesitamos especificar tipo de instancia ni par de claves, ya que Fargate gestiona estos recursos automáticamente. 🎛️

4. **Finalizar y Crear**:
   - Revisamos las configuraciones y hacemos clic en "Crear" para finalizar el proceso. 📝

¡Listo! Ahora tienes un cluster de ECS configurado y listo para desplegar tus contenedores. Excelente trabajo. ¡Estamos cada vez más cerca del despliegue final! 🚢💪

## Paso 8: Definir una tarea de ECS

LabRole para rol de tarea y rol de ejecución.

tamaño de la tarea 2 vcpu y 4 gb de ram.

cargamos la variables de entorno copaimos las credenciales de la base de datos en el contenedor. 

## Paso 9: Crear un Servicio de ECS

