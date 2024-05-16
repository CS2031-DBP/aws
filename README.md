# AWS - DBP CS2031 🌐

¡Bienvenidos a la semana 8 de tu curso favorito, Desarrollo Basado en Plataformas CS2031 🎓! Esta semana, daremos un paso importante: aprenderemos a usar la "nube" para hacer que nuestras aplicaciones sean accesibles desde cualquier parte del mundo 🌍.

## Objetivos 🎯

Desplegar un proyecto backend en la nube usando Amazon Web Services (AWS) 🚀. Empleando contenedores Docker, en los servicios ECR (Elastic Container Registry) y ECS (Elastic Container Service) de AWS. Usando Github Actions para CI/CD.

## Masterclass Auditorio 🎤

Nuestro asistente de cátedra (T.A) Gabriel Romero, brindó una masterclass en el auditorio de CS2031 en UTEC, donde dio una introducción a la nube y AWS. Si te la perdiste, no te preocupes, aquí te dejamos un resumen de lo que aprendimos en la masterclass 📚.

Enlace de la masterclass: [Intro AWS]()

## ¿Qué es la nube? ☁️

Hasta ahora, hemos desarrollado aplicaciones que corren en nuestra computadora o en un servidor local. Sin embargo, esto tiene limitaciones: si queremos que nuestra aplicación sea accesible para otras personas, necesitamos un servidor que esté siempre encendido y conectado a internet 🌐.

Es aquí donde entra la "nube". En lugar de tener un servidor físico, podemos usar un servicio en internet que nos provee de servidores virtuales, bases de datos, almacenamiento y más. Esto nos permite desplegar nuestras aplicaciones de manera sencilla y escalable 🚀.

Cuando hablamos de "desplegar en la nube", nos referimos a subir nuestras aplicaciones a un servicio en internet que las mantiene funcionando y accesibles para cualquier persona en cualquier lugar. Esto es ideal si quieres que tu aplicación pueda ser usada por personas de todo el mundo 🌐.

Desplegar en la nube, como lo haremos con AWS, tiene muchas ventajas:

1. **Accesibilidad:** Tu aplicación puede ser accesible desde cualquier lugar 🌎.
2. **Escalabilidad:** Si tu aplicación se hace popular, la nube permite aumentar su capacidad fácilmente.
3. **Seguridad:** Los proveedores de la nube invierten mucho en seguridad para proteger tus datos.
4. **Costo:** Generalmente es más barato usar la nube que mantener tus propios servidores.

## ¿Por qué AWS? 🤔

Amazon Web Services (AWS) es uno de los proveedores de servicios en la nube más grandes y confiables. Ofrece desde potencia de cómputo hasta almacenamiento de bases de datos y entrega de contenido, ayudando a las empresas a crecer y escalar 📈.

UTEC tiene una alianza con AWS Academy, dándonos acceso a recursos exclusivos. Si te interesa aprender cómo aprovechar al máximo estos servicios, ¡inscríbete en el curso de Cloud Computing CS2032 con Geraldo Colchado! 📚

Este demo es solo el principio. Vamos a desplegar una aplicación usando contenedores Docker en los servicios ECR y ECS de AWS. ¡Vamos a ello! 🚀

## AWS Academy 🎓

AWS Academy es un programa global de Amazon que colabora con universidades para preparar a estudiantes como tú para carreras en la nube. UTEC es parte de este prestigioso programa en Perú, ofreciéndote recursos exclusivos para que aprendas y te certifiques en tecnologías de AWS.

### ¡Comienza tu aventura en la nube! 🌟

- **Activa tu cuenta:** Revisa tu correo y activa tu cuenta de AWS Academy. Esto te dará acceso no solo a recursos educativos, sino también a créditos gratuitos para explorar servicios de AWS en prácticas reales.

- **Curso Academy Cloud Foundations (`AFC`):** Ya tienes acceso a este curso en AWS Academy. Aprenderás lo esencial sobre la nube y cómo utilizar los servicios de AWS de manera efectiva. Es una introducción perfecta para todos, independientemente de tu nivel de experiencia previo.

### Beneficios de certificarte 🚀

- **Potencia tu carrera:** Las certificaciones de AWS son altamente valoradas en el sector tecnológico. Obtener una puede abrirte muchas puertas en tu futuro profesional.
- **Capacitación sin costo:** Aprovecha esta capacitación gratuita para elevar tus habilidades técnicas y destacarte en el ámbito profesional.

### ¡No pierdas esta oportunidad! 🌟

Ser parte de AWS Academy es una gran ventaja en tu educación y carrera. Asegúrate de aprovechar al máximo todos los recursos y oportunidades que esto te brinda. ¡Es tu momento de brillar en el mundo de la computación en la nube!

## ¡Manos a la obra! 🛠️

¡Es hora de desplegar nuestra aplicación en la nube! Sigue los pasos que te mostraremos en el demo para subir tu proyecto a AWS. ¡Vamos a ello! 🚀

### Indice 📋

Hemos creado una guía paso a paso para que puedas seguir el demo de despliegue en AWS. ¡Sigue los pasos y despliega tu aplicación en la nube! 🌐

1. [Introducción a Cloud](./docs/01-intro-cloud.md)
2. [AWS Academy](./docs/02-aws-academy.md)
3. [Docker](./docs/03-docker-ec2.md)