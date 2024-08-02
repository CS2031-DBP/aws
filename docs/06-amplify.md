# 🚀 Deployando nuestro Frontend en AWS Amplify

Hemos aprendido cómo desplegar cualquier aplicación en AWS utilizando contenedores Docker. Ahora, vamos a desplegar nuestro frontend en **AWS Amplify**. 🌐

## 🌟 ¿Qué es AWS Amplify?

AWS Amplify es un servicio que permite construir aplicaciones fullstack utilizando tu framework preferido. Puedes usar AWS Amplify con frameworks web y móviles populares como JavaScript, Flutter, Swift y React. Con Amplify, puedes construir, conectar y alojar aplicaciones fullstack en AWS de manera sencilla y eficiente. 

### ⚙️ Características de AWS Amplify

- **Soporte para hosting SSR/SSG/ISR**: Despliega aplicaciones Next.js, Nuxt, React, Vue.js, Angular y más simplemente conectando tu repositorio Git.
- **Iteraciones más rápidas con sandboxes por desarrollador**: Los sandboxes en la nube proporcionan alta fidelidad y tiempos de despliegue más rápidos para iteraciones locales ágiles.
- **Ramas fullstack sin configuración**: Despliegues fullstack desde tu rama de Git. Auto-despliega ramas de Git para configurar entornos de staging, desarrollo y producción.
- **Interfaz gráfica para gestionar tus datos**: Gestiona los datos de tu aplicación, usuarios y grupos, y archivos en una única consola.

## 🎯 ¿Por qué usar Amplify?

Amplify nos permite desplegar aplicaciones web de forma sencilla, rápida y segura, ocupándose de todo el proceso de despliegue, desde la creación de un entorno de desarrollo hasta la configuración de un dominio personalizado.

- **Simplicidad**: Nos permite enfocarnos en el desarrollo de nuestra aplicación, dejando el despliegue en manos de Amplify.
- **Costo accesible**: Solo pagamos por los recursos que utilizamos, sin costos fijos. Esto permite escalar nuestra aplicación fácilmente y pagar solo por lo que utilizamos. Si tu web solo recibe 100 visitas al mes, solo pagarás por esas 100 visitas.

> **Nota**: No veremos el tema del dominio personalizado debido a las limitaciones de la cuenta gratuita de AWS Academy. Sin embargo, puedes seguir la [documentación oficial de AWS](https://docs.aws.amazon.com/amplify/index.html) para configurar un dominio personalizado. También puedes canjear un dominio gratuito mediante el GitHub Student Pack.

## 🛠️ Paso 1: Crear un nuevo proyecto en Amplify

**Nota:** Asegúrate de tener una cuenta de AWS y haber iniciado sesión en la consola de AWS, además de tener un proyecto de React en tu repositorio de GitHub.

1. Accede a la consola de AWS y busca el servicio de **Amplify**.
2. Haz clic en **Crear nueva aplicación**.
3. Selecciona desde dónde quieres desplegar tu aplicación. En este caso, selecciona **GitHub**.
4. Haz clic en **Conectar con GitHub** y sigue los pasos para conectar tu cuenta de GitHub con AWS Amplify.
5. Una vez conectada tu cuenta de GitHub, selecciona el repositorio de tu proyecto de React. En la barra de búsqueda, escribe el nombre de tu repositorio y haz clic. Luego, selecciona la **rama** de tu proyecto que quieres desplegar.
6. Haz clic en **Siguiente**. Te aparecerá una pantalla con la configuración de tu proyecto. Haz clic en **Configuración avanzada** y en el apartado de **Variables de entorno**, añade las variables de entorno que necesites para tu proyecto. Por ejemplo, si necesitas una variable de entorno llamada `API_URL`, añádela en este apartado.
7. Haz clic en **Siguiente** y después en **Guardar y desplegar**.
8. Amplify comenzará a desplegar tu aplicación. Una vez finalizado el despliegue, haz clic en el enlace que te proporciona Amplify para acceder a tu aplicación.

![Amplify](../media/AWS-AMPLIFY.gif)

¡Felicidades, has desplegado tu aplicación en AWS Amplify! 🚀

> **Nota:** El enlace es estático, por lo que si haces cambios en tu proyecto, estos no se verán reflejados en el enlace hasta que no hagas un nuevo despliegue. Para actualizar tu web, simplemente haz un push a tu rama principal de GitHub y Amplify se encargará de desplegar los cambios automáticamente. La integración continua está activada por defecto para la rama que hayas seleccionado.