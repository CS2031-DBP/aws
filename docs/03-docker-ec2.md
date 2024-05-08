# Docker: el contenedor mágico 🐳

Hasta ahora, hemos utilizado Docker de forma local para evitar instalar PostgreSQL en nuestros dispositivos. Sin embargo, Docker es una herramienta poderosa que facilita el empaquetado y despliegue de nuestras aplicaciones en cualquier entorno.

Todos conocemos el dicho: "Funciona en mi máquina". Docker soluciona este problema permitiéndonos empaquetar nuestra aplicación y todas sus dependencias en un contenedor. Esto asegura que se ejecute igual en cualquier lugar.

Creamos nuestro contenedor Docker con todo lo necesario, ya sea una aplicación web, una base de datos o un servidor. Luego, podemos subirlo a la nube y ejecutarlo donde sea necesario. Así es como Docker estandariza el entorno de ejecución de nuestras aplicaciones, permitiendo un despliegue rápido y confiable.

## ¿Qué es Docker? 🐳

Docker es una plataforma de código abierto desarrollada en Go. Automatiza el despliegue de aplicaciones en contenedores de software, que son unidades estándar que empaquetan el código y sus dependencias. Esto permite que la aplicación se ejecute de manera rápida y confiable en cualquier entorno.

## ¿Por qué usar Docker? 🤔

Existen alternativas a Docker, como Vagrant, pero Docker sigue siendo la herramienta más popular y utilizada para contenedores. Las razones de su popularidad incluyen:

1. **Portabilidad:** Los contenedores de Docker pueden ejecutarse en cualquier lugar, desde una computadora personal hasta la nube o un servidor, facilitando la transferencia de aplicaciones.

2. **Escalabilidad:** Docker usa los recursos de manera eficiente, permitiendo la ejecución de múltiples contenedores en un solo servidor. Ideal para aplicaciones que necesitan escalar rápidamente.

3. **Rapidez:** Docker opera más rápido que muchas alternativas, pues los contenedores se ejecutan directamente en el sistema operativo del host, sin necesidad de una máquina virtual.

4. **Facilidad de uso:** Docker ofrece una interfaz de línea de comandos simple y clara para crear, ejecutar y gestionar contenedores.

## ¿Cómo funciona Docker? 🤯

En la plataforma CS2031, ofrecemos una masterclass sobre Docker. Aquí te dejamos el enlace para que puedas aprender más sobre esta herramienta.

Enlace a la masterclass: [Masterclass Docker](https://youtu.be/OJa-icK4dWE)

## ¿Qué es un contenedor? 📦

Un contenedor es una unidad estándar de software que empaqueta el código y todas sus dependencias. Esto incluye las bibliotecas, herramientas y configuraciones necesarias para que la aplicación se ejecute correctamente. Los contenedores son portátiles y se pueden ejecutar en cualquier entorno, desde una computadora personal hasta la nube.

En palabras sensillas, un contenedor es como una caja que contiene todo lo necesario para que una aplicación funcione. Docker nos permite crear, ejecutar y gestionar estos contenedores de manera sencilla y eficiente.

Por ejemplo, si queremos ejecutar una base de datos PostgreSQL, ejecutamos el siguiente comando:

```bash
docker run -d -p 5432:5432 --name postgres postgres
```

Este comando descarga la imagen de PostgreSQL, crea un contenedor con el nombre `postgres` y lo ejecuta en segundo plano. Además, mapea el puerto 5432 del contenedor al puerto 5432 del host, permitiendo la conexión a la base de datos.

## ¿Cómo crear una imagen de Docker? 🖼

Hemos estado utilizando la terminal para ejecutar comandos de Docker, pero también podemos crear contenedores a través de un archivo de configuración llamado `Dockerfile`. Este archivo define las instrucciones necesarias para construir una imagen de Docker, que luego se puede utilizar para crear contenedores.

Es muy util crear un `Dockerfile` para nuestra aplicación, ya que nos permite definir el entorno de ejecución de forma reproducible. Por ejemplo, podemos especificar la versión de Python, instalar las dependencias necesarias y copiar el código fuente de nuestra aplicación.

A continuación, se muestra un ejemplo de un `Dockerfile` para una aplicación Flask:

```Dockerfile
# Usamos la imagen oficial de Python
FROM python:3.9

# Establecemos el directorio de trabajo
WORKDIR /app

# Copiamos los archivos de la aplicación
COPY . /app

# Instalamos las dependencias
RUN pip install -r requirements.txt

# Exponemos el puerto 5000
EXPOSE 5000

# Ejecutamos la aplicación
CMD ["python", "app.py"]
```

Este `Dockerfile` define una imagen de Docker que utiliza Python 3.9, instala las dependencias de la aplicación, expone el puerto 5000 y ejecuta el archivo `app.py`. Luego, podemos construir la imagen con el siguiente comando:

```bash
docker build -t myapp .
```

Este comando crea una imagen de Docker con el nombre `myapp` a partir del `Dockerfile` en el directorio actual. Luego, podemos ejecutar un contenedor con esta imagen:

```bash
docker run -d -p 5000:5000 myapp
```

Este comando crea un contenedor con el nombre `myapp` y lo ejecuta en segundo plano. Además, mapea el puerto 5000 del contenedor al puerto 5000 del host, permitiendo el acceso a la aplicación.

### Dockerfile: el archivo de configuración de Docker 📄

El `Dockerfile` es un archivo de texto que contiene las instrucciones necesarias para construir una imagen de Docker. Estas instrucciones se dividen en comandos, como `FROM`, `RUN`, `COPY`, `EXPOSE` y `CMD`, que definen cómo se construye y ejecuta la imagen.

- **FROM:** Especifica la imagen base que se utilizará para construir la nueva imagen. Por ejemplo, `FROM python:3.9` utiliza la imagen oficial de Python 3.9 como base.

- **WORKDIR:** Establece el directorio de trabajo dentro del contenedor. Todas las instrucciones siguientes se ejecutan en este directorio. Por ejemplo, `WORKDIR /app` establece el directorio de trabajo en `/app`.

- **COPY:** Copia archivos desde el host al contenedor. Por ejemplo, `COPY . /app` copia todos los archivos del directorio actual al directorio `/app` del contenedor.

- **RUN:** Ejecuta comandos en el contenedor durante la construcción de la imagen. Por ejemplo, `RUN pip install -r requirements.txt` instala las dependencias de la aplicación.

- **EXPOSE:** Expone un puerto del contenedor al host. Por ejemplo, `EXPOSE 5000` expone el puerto 5000 del contenedor.

- **CMD:** Especifica el comando que se ejecutará cuando se inicie el contenedor. Por ejemplo, `CMD ["python", "app.py"]` ejecuta el archivo `app.py` con Python.

Estas son solo algunas de las instrucciones disponibles en un `Dockerfile`. Puedes consultar la documentación oficial de Docker para obtener más información sobre cómo crear imágenes de Docker.

## Imagen vs Contenedor 🖼 vs 📦

Explorando el mundo de Docker, es fundamental entender la diferencia entre dos términos esenciales: **imagen** y **contenedor**. Vamos a aclarar estos conceptos con un enfoque pedagógico:

- **Imagen:** 🖼 Una imagen de Docker es como una receta de cocina. Contiene todos los ingredientes (código, bibliotecas, herramientas) y las instrucciones (configuraciones) necesarios para preparar un plato (aplicación). Las imágenes son paquetes de solo lectura que sirven como plantillas para crear contenedores. Piensa en ellas como un molde que tiene todo listo para empezar a trabajar.

- **Contenedor:** 📦 Un contenedor es como un plato cocinado según la receta que proporciona la imagen. Es una instancia en ejecución de una imagen, funcionando en un entorno aislado. Al ser ligeros y compartir el núcleo del sistema operativo del host, los contenedores se inician rápidamente y usan menos recursos que una máquina virtual completa. Son la realización práctica de lo que la imagen ha definido, operando de manera autónoma del resto de tu sistema.

Resumiendo, **la imagen es la plantilla estática**, mientras que **el contenedor es su ejecución dinámica**. Utilizamos imágenes para crear múltiples contenedores, asegurando que cada contenedor se inicie con el mismo ambiente y configuración.

## DockerHub: el repositorio de imágenes de Docker 🐳

DockerHub puede ser comparado con el GitHub de las imágenes de Docker. Es un repositorio en la nube que permite a los desarrolladores almacenar y compartir imágenes de Docker. Aquí puedes encontrar desde imágenes oficiales para aplicaciones populares como PostgreSQL, MySQL y Redis, hasta imágenes personalizadas creadas por otros desarrolladores.

Para utilizar una imagen de DockerHub, simplemente usamos el comando `docker pull`. Por ejemplo, para obtener la imagen de PostgreSQL, ejecutarías:

```bash
docker pull postgres
```

Este comando descarga la imagen de PostgreSQL desde DockerHub directamente a tu máquina. Posteriormente, puedes usar esta imagen para crear un contenedor que ejecutará la base de datos.

Además, si desarrollas una imagen propia que podría ser útil para otros, puedes subirla a DockerHub. Solo necesitas crear una cuenta y seguir unos simples pasos para compartir tu trabajo con la comunidad global.

## Cloud Computing y Docker ☁️

Docker es una herramienta esencial en el ámbito del cloud computing. Plataformas como AWS, Azure y Google Cloud ofrecen servicios que facilitan la ejecución de contenedores Docker en la nube, permitiendo una escalabilidad impresionante sin la carga de gestionar la infraestructura subyacente.

Estos servicios pueden ser completamente sin servidor (Serverless) o basados en FaaS (Function as a Service), donde no necesitas preocuparte por el servidor en sí. Simplemente subes tus imágenes a DockerHub o directamente a la nube, y la plataforma se encarga de todo lo demás.

A lo largo de este curso, aprenderás cómo desplegar y escalar aplicaciones utilizando Docker y tecnologías de cloud computing. Esto te preparará para enfrentar desafíos reales en entornos de producción, centrando tus esfuerzos en el desarrollo y mejoramiento de aplicaciones, más allá de la infraestructura que las soporta. Esto será fundamental en tu próximo curso CS2032 sobre Cloud Computing.

## Ejemplo práctico: Docker en AWS EC2 🚀

Para finalizar, vamos a realizar un ejemplo práctico de cómo ejecutar un contenedor Docker en una instancia de AWS EC2. Este es un escenario común en el mundo real, donde desplegamos aplicaciones en la nube utilizando Docker para garantizar la portabilidad y escalabilidad.

### Requisitos previos 📋

- Haber activado tu acceso a AWS Academy y obtenido tus credenciales de acceso.
- Tener Docker instalado en tu propia máquina.

### Preparando nuestra aplicación 🚀

1. **Crear una aplicación FastAPI**: Primero, crea un archivo `app.py` con el siguiente código para una API simple de FastAPI:
   ```python
   from fastapi import FastAPI

   app = FastAPI()

   @app.get("/")
   def read_root():
       return {"Hello": "World"}

   if __name__ == "__main__":
       import uvicorn
       uvicorn.run(app, host="0.0.0.0", port=8000)
   ```

2. **Crear un archivo `Dockerfile`**: Ahora, crea un `Dockerfile` que defina las instrucciones para construir una imagen de Docker para nuestra aplicación:
   ```Dockerfile
   FROM python:3.9

   WORKDIR /app

   COPY . /app

   RUN pip install fastapi uvicorn

   EXPOSE 8000

   CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
   ```

3. **Construir la imagen de Docker**: Utiliza el siguiente comando para construir la imagen de Docker:
   ```bash
   docker build -t fastapi-app .
   ```

4. **Ejecutar el contenedor**: Ejecuta un contenedor con la imagen de Docker en tu máquina:
   ```bash
   docker run -d -p 8000:8000 fastapi-app
   ```

    El comando anterior ejecuta un contenedor en segundo plano y mapea el puerto 8000 del contenedor al puerto 8000 de tu máquina.  

5. **Probar la aplicación**: Abre un navegador y navega a `http://localhost:8000` para ver el mensaje "Hello World" de la API FastAPI.

6. **Terminar el contenedor**: Para detener y eliminar el contenedor, primero tenemos que obtener su ID con el comando `docker ps` y luego ejecutar:
   ```bash
   docker stop <container-id>
   docker rm <container-id>
   ```

7. **Subir la imagen a DockerHub**: Finalmente, sube tu imagen a DockerHub para poder utilizarla desde cualquier lugar.
   - Crea una cuenta en [DockerHub](https://hub.docker.com/) si no tienes una.
   - Crea un repositorio y copia su nombre.
   - Inicia sesión en DockerHub desde la terminal:
     ```bash
     docker login
     ```
    Si tienes problemas con la contraseña, puedes generar un token de acceso en la página de DockerHub. Para ello ve a `Account Settings` -> `Security` -> `New Access Token`. Luego, utiliza el token en lugar de la contraseña.

   - Etiqueta tu imagen con el nombre del repositorio:
     ```bash
     docker tag fastapi-app <nombre-usuario>/<nombre-repositorio>:latest
     ```
   - Sube la imagen a DockerHub:
     ```bash
     docker push <nombre-usuario>/<nombre-repositorio>:latest
     ```
   - Verifica que la imagen se haya subido correctamente.

### Desplegando el contenedor en AWS EC2 🌐

Para ejecutar nuestro contenedor en la nube, sigue estos pasos:

1. **Crear una instancia de AWS EC2** 🖥
   - Inicia sesión en la consola de AWS y navega a EC2.
   - Haz clic en "Launch Instance" para crear una nueva instancia.
   - Selecciona una AMI adecuada, como `Ubuntu Server 20.04 LTS`.
   - Configura la instancia según tus necesidades y lánzala.

2. **Conectar a la instancia** 🔗
    - Copia la dirección IP pública de la instancia.
    - Conéctate a la instancia mediante SSH:
      ```bash
      ssh -i tu_clave.pem ubuntu@ip-publica
      ```
    - Actualiza los paquetes del sistema:
      ```bash
      sudo apt update
      sudo apt upgrade
      ```

3. **Instalar Docker en la instancia** 🐳
    - Instala Docker si aún no está instalado:
      ```bash
      sudo apt install docker.io
      ```
    - Asegúrate de que Docker se ejecute al inicio:
      ```bash
      sudo systemctl enable --now docker
      ```

4. **Ejecutar el contenedor en EC2**:
    - Descarga la imagen de DockerHub:
      ```bash
      docker pull <nombre-usuario>/<nombre-repositorio>:

latest
      ```
    - Ejecuta la imagen como un contenedor:
      ```bash
      docker run -d -p 80:8000 <nombre-usuario>/<nombre-repositorio>:latest
      ```

5. **Accede a la aplicación** 🌍:
    - Abre un navegador y navega a la dirección IP pública de tu instancia EC2 para acceder a la aplicación desplegada.

## Conclusión 🎉

Al final de este tutorial, habrás aprendido a desplegar una aplicación usando Docker y AWS EC2, una habilidad esencial en el mundo del desarrollo moderno y la computación en la nube. ¡Felicidades por alcanzar este hito!