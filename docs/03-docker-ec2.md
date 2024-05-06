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