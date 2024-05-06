# Consola AWS - AWS Academy 🎓

AWS Academy es un programa global de Amazon que colabora con instituciones educativas para preparar a estudiantes como tú para carreras en la tecnología de la nube. UTEC, como parte de este destacado programa en Perú, te ofrece recursos exclusivos para que aprendas y obtengas certificaciones en tecnologías de AWS.

## Activación de cuenta 🚀

Revisa tu correo y activa tu cuenta de AWS Academy siguiendo estos pasos:

1. **Correo de Invitación**: Busca un correo con el asunto "Invitación al curso" o "Course Invitation" enviado por "AWS Academy".
2. **Activación**: Haz clic en el enlace de activación y regístrate con tu correo institucional.
3. **Spam**: Si no encuentras el correo, revisa tu bandeja de spam.

## Plataforma AWS Academy 🌐

AWS Academy utiliza CANVAS Instructure como su plataforma. Aquí encontrarás:

- **Cursos Disponibles**: Acceso a cursos como "AWS Academy Cloud Foundations" (`AFC`), ideal para introducirte en la nube y los servicios de AWS.
- **Recursos Educativos**: Materiales y herramientas para tu aprendizaje.
- **Evaluaciones**: Pruebas para medir tu progreso.

Aunque no es obligatorio completar el curso para DBP, te recomendamos hacerlo para maximizar tu aprovechamiento de los recursos ofrecidos.

## Sandbox de prácticas 🧪

Activa tu laboratorio de prácticas en AWS Academy siguiendo estos pasos:

1. **Acceso a la Plataforma**: Ingresa a AWS Academy.
2. **Sandbox Environment**: Dirígete a esta sección y haz clic en "Start Lab" para iniciar.
![Sandbox](/media/sandbox.png)
3. **Activación del Laboratorio**: Observa cómo el círculo se torna verde, indicando que el laboratorio está activo. Esto puede tardar unos minutos.
![Sandbox](/media/sandbox-color-green.png)
4. **Acceso a la Consola**: Una vez activo, haz clic en el círculo verde para ir a la consola de AWS.
![Sandbox](/media/aws-console-home.png)

## Credenciales de acceso 🔑

Obtén tus credenciales de acceso a la consola de AWS así:

1. **Detalles del Sandbox**: En la sección "AWS Details" del sandbox encontrarás tus credenciales de acceso.
![Credenciales](/media/sandbox-details.png)
2. **Descarga de Credenciales**: Haz clic en "Download Credentials" para obtener un archivo con tus credenciales. Selecciona "Download PEM" para obtener la clave privada.

Recuerda que estas credenciales se mantendrán activas solo durante la duración del sandbox. Serán desactivadas al finalizar.

### Advertencia ⚠️

Las credenciales son personales y temporales. No las compartas y asegúrate de cerrar sesión al terminar de usarlas para mantener la seguridad.

## Créditos de AWS Academy 💳

Como parte de AWS Academy, tienes acceso a 100 USD en créditos de AWS para que puedas explorar los servicios de AWS en prácticas reales. ¡Aprovéchalos al máximo!

Los sandbox de prácticas tienen una duración limitada de 3 a 4 horas. Asegúrate de aprovechar al máximo tu tiempo y cerrar sesión al terminar. Hay servicios que pueden seguir consumiendo créditos aunque no estés activamente en la consola.

## Explorando la consola de AWS con EC2 🔍

Sumérgete en la consola de AWS y descubre sus servicios para maximizar tu aprendizaje en la nube. Comenzaremos con el servicio EC2, que te permite lanzar y gestionar instancias virtuales en la nube de AWS. 

### ¿Qué es Amazon EC2? 🤔

Amazon Elastic Compute Cloud (EC2) proporciona capacidad informática segura y escalable en la nube. Funciona como una computadora virtual en la nube a la que puedes acceder vía SSH, como si fuese tu propia computadora, pero ubicada en el internet. Esto te permite:

- **Configuración a la Medida**: Elige el sistema operativo, la memoria y el almacenamiento según tus necesidades.
- **Sin Interfaz Gráfica**: Accede únicamente mediante la terminal, ideal para aplicaciones que no requieren interfaz gráfica, como servidores web o bases de datos.

#### Concepto de Máquina Virtual 🖥️

Una máquina virtual es un software que simula un sistema informático, permitiéndote ejecutar programas como si estuvieras en una computadora física. A diferencia de los contenedores, que son más ligeros y rápidos, las máquinas virtuales ofrecen un entorno completamente aislado y seguro con su propio sistema operativo.

### Lanzando una instancia EC2 🚀

Exploraremos el proceso de lanzar una instancia EC2 utilizando un ejemplo práctico donde desplegaremos una API usando FastAPI, un moderno framework de Python para crear APIs eficientes y seguras. Aquí lo que necesitaremos:

1. **Sistema Operativo**: Preferiblemente Ubuntu 20.04 LTS.
2. **Puertos Abiertos**: Necesitaremos los puertos 80 (HTTP) y 443 (HTTPS).
3. **Software Requerido**: Python versión 3.8 o superior.
4. **Librerías**: Instalación de FastAPI, Uvicorn entre otras.
5. **Código**: Dispondremos de un código de ejemplo para la API.

### Guía de lanzamiento 📋

Nuestro primer paso será lanzar una instancia EC2 siguiendo estos pasos:

1. **Ingreso a la Consola**: Ingresa a la consola de AWS y dbusca el servicio EC2.
![EC2](/media/search-ec2-aws-console.png)

2. **Selección de AMI**: Los AMI (Amazon Machine Image) son plantillas preconfiguradas con sistemas operativos y software. De esta manera no nos preocupamos por instalar el sistema operativo desde cero ni dependencias adicionales como lenjuages de programación o librerías.

![AMI](/media/aws-console-ami.png)

3. **Selección de busqueda pública**: Cambiamos la busqueda a AMIs públicas para buscar una imagen de Ubuntu.

![AMI](/media/aws-console-ami-public.png)

4. **Filtrado de AMIs**: Filtramos las AMIs públicas por "Cloud9Ubuntu-2024". Nos aparecerán multiples opciones, seleccionamos la primera la del mes más reciente.

![AMI](/media/AMI-AWS-CONSOLE-SHOW.gif)

5. **Lanzamiento de la instancia**: Seleccionamos la AMI y lanzamos la instancia. 

6. **Nombre y etiquetas**: Asignamos un nombre a la instancia y etiquetas para identificarla. En este caso, usaremos "FastAPI-Instance".

7. **Tipo de instancia**: Seleccionamos el tipo de instancia. Para este ejemplo, usaremos una instancia `t2.micro` que es gratuita.

8. **Par de claves**: Selecionamos `vockey` como par de claves para acceder a la instancia.

9. **Grupo de seguridad**: Creamos un nuevo grupo de seguridad y permitimos el tráfico en SSH y HTTP desde cualquier lugar.

10. **Configurar almacenamiento**: Cambiamos el almacenamiento a 15 GB.

11. **Revisión y lanzamiento**: Revisamos la configuración que hemos seleccionado y lanzamos la instancia.

![Crear EC2](/media/AWS-CONSOLE-CREATE-EC2.gif)

12. **Entramos a la instancia**: Una vez lanzada la instancia, ingresamos a ella y copiamos la dirección IP pública.

![Entrar a la instancia](/media/AWS-CONSOLE-INSTANCE-AFTER-CREATE-EC2.gif)

13. **Conectamos a la instancia**: Conectamos a la instancia mediante SSH y actualizamos los paquetes.

Ahora para conectarnos a la instancia, utilizamos el siguiente comando en Git Bash o en la terminal de Linux:

```bash
ssh -i labsuser.pem ubuntu@<IP-PUBLICA>
```
Una vez conectados nos aparecerá un mensaje de advertencia, escribimos `yes` y presionamos `Enter`.

![Conectar a la instancia](/media/SSH-GITBASH-FIRST-CONECT.png)

Nos hemos conectado a nuestro EC2, ahora actualizamos los paquetes con el siguiente comando:

```bash
sudo apt update
```

14. **Instalamos FastAPI**: Instalamos FastAPI y Uvicorn con el siguiente comando, ya que Python ya viene instalado en la AMI de Ubuntu.:

```bash
pip3 install fastapi uvicorn
```

15. **Creamos un archivo de código**: Creamos un archivo usando `nano main.py` con el siguiente código:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello World"}

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

Copia y pega el código en el archivo. Para cerrar nano y guardar los cambios, presionamos `Ctrl + o`, `Enter` y `Ctrl + x`.

16. **Grupos de seguridad**: Para que nuestra API sea accesible, debemos abrir el puerto 8000. Accedemos a la consola de AWS y localizamos el grupo de seguridad de nuestra instancia. Al crear el EC2, seleccionamos un grupo de seguridad llamado `launch-wizard-1`. Añadimos una regla de entrada para el puerto 8000, permitiendo tráfico desde cualquier dirección.

![Grupos de seguridad](/media/AWS-GROUP-SECURITY-PORT8000-EC2.gif)

18. **Ejecutamos la API**: Ejecutamos la API en el puerto 8000 con el siguiente comando:

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

19. **Accedemos a la API**: En Postman seleccionamos el método `GET` y colocamos la dirección IP pública de la instancia con el puerto 8000. Deberíamos ver el mensaje "Hello World".

La url de la API sería `http://<IP-PUBLICA>:8000`.

![Postman](/media/POSTMAN-EC2-FASTAPI-GET.png)

¡Listo! Hemos lanzado una instancia EC2 en AWS y desplegado una API con FastAPI. Ahora que conoces un poco de la dinámica de EC2, tenemos que conocer el rol de Docker en la nube y cómo podemos desplegar aplicaciones con contenedores.