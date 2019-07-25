---
title: Cómo implementar el módulo OPC Vault desde cero - Azure | Microsoft Docs
description: Implementación de OPC Vault desde cero.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 73588d8c84525cf70de9fac312b0a485055b0a2c
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605703"
---
# <a name="deploy-opc-vault-from-scratch"></a>Implementación de OPC Vault desde cero

OPC Vault es un microservicio que puede configurar, registrar y administrar el ciclo de vida de los certificados para aplicaciones cliente y servidor de OPC UA en la nube. Este artículo le muestra cómo implementar OPC Vault desde cero.

## <a name="configuration-and-environment-variables"></a>Variables de configuración y entorno

La configuración del servicio se almacena mediante los adaptadores de configuración de ASP.NET Core, en appsettings.ini. El formato INI permite almacenar valores en un formato legible, junto con los comentarios.
La aplicación también admite la inserción de variables de entorno, tales como credenciales y detalles de red. (Esta sección se titula originalmente "Tareas: configuración y variables de entorno").

El archivo de configuración del repositorio hace referencia a algunas variables de entorno que deben crearse al menos una vez. Dependiendo de su sistema operativo y el IDE, hay varias formas de administrar las variables de entorno:

- Para los usuarios de Windows, el script env-vars-setup.cmd debe prepararse y ejecutarse solo una vez. Cuando se ejecute, la configuración persistirá en las sesiones de terminal y los reinicios.

- Para los entornos Linux y OSX, el script env-vars-setup debe ejecutarse cada vez que se abre una nueva consola. Según el sistema operativo y el terminal, existen formas de conservar los valores de forma global. Para obtener más información, estas páginas deberían serle útiles:

  - https://stackoverflow.com/questions/13046624/how-to-permanently-export-a-variable-in-linux
  
  - https://stackoverflow.com/questions/135688/setting-environment-variables-in-os-x
  
  - https://help.ubuntu.com/community/EnvironmentVariables
  

- Visual Studio: Las variables de entorno también se pueden configurar desde Visual Studio, en la opción del panel izquierdo "Propiedades del proyecto"; una vez en ella, seleccione "Propiedades de configuración" y "Entorno" para acceder a una sección donde pueda agregar varias variables.

- IntelliJ Rider: Las variables de entorno se pueden configurar en cada configuración de ejecución, de manera similar a IntelliJ IDEA https://www.jetbrains.com/help/idea/run-debug-configuration-application.html.

## <a name="run-and-debug-with-visual-studio"></a>Ejecutar y depurar con Visual Studio

Visual Studio le permite abrir rápidamente la aplicación sin tener que usar un indicador de comandos y sin configurar nada fuera del IDE.

Para ejecutar y depurar la aplicación usando Visual Studio:

1. Abra la solución utilizando el archivo `iot-opc-gds-service.sln`.

1. Cuando se cargue la solución, haga clic derecho en el proyecto `WebService`, seleccione y vaya a la sección `Debug`.

1. En la misma sección, defina las variables de entorno requeridas.

1. Presione **F5**, o el icono **Ejecutar**. Visual Studio debería abrir el explorador mostrando el estado del servicio en formato JSON.

## <a name="run-and-debug-with-intellij-rider"></a>Ejecutar y depurar con IntelliJ Rider

1. Abra la solución utilizando el archivo `iot-opc-gds-service.sln`.

1. Cuando se haya cargado la solución, vaya a `Run > Edit Configurations` y cree una nueva configuración de `.NET Project`.

1. En la configuración, seleccione el proyecto de WebService.

1. Guarde la configuración y ejecute la que se creó desde la barra de herramientas IDE.

1. Debería ver los mensajes de arranque del servicio en la ventana "Ejecutar" de IntelliJ con detalles como la dirección URL donde se ejecuta el servicio web además de los registros del servicio.

## <a name="build-and-run-from-the-command-line"></a>Compilar y ejecutar desde la línea de comandos

La carpeta de scripts contiene algunos scripts para realizar las tareas frecuentes:

- `build`: compila todos los proyectos y ejecuta las pruebas.

- `compile`: compila todos los proyectos.

- `run`: compila los proyectos y ejecuta el servicio, el cual solicita privilegios elevados en Windows para ejecutar el servicio web.

Asimismo, los scripts comprueban la configuración de las variables de entorno. Puede configurar las variables de entorno globalmente en su sistema operativo, o usar el script "env-vars-setup" en la carpeta de scripts.

### <a name="sandbox"></a>Espacio aislado

Los scripts asumen que configuró su entorno de desarrollo con .NET Core y Docker. Puede evitar instalar .NET Core e instalar solo Docker y usar el parámetro de línea de comando `--in-sandbox` (o la forma corta `-s`), por ejemplo:

- `build --in-sandbox`: ejecuta la tarea de compilación dentro de un contenedor Docker (forma corta `build -s`).

- `compile --in-sandbox`: ejecuta la tarea de compilación dentro de un contenedor Docker (forma corta `compile -s`).

- `run --in-sandbox`: inicia el servicio dentro de un contenedor Docker (forma corta `run -s`).

Las imágenes de Docker que se usan para el espacio aislado se hospedan en Docker Hub [aquí](https://hub.docker.com/r/azureiotpcs/code-builder-dotnet).

## <a name="package-the-application-to-a-docker-image"></a>Empaquetar la aplicación en una imagen de Docker

La carpeta `scripts` incluye una subcarpeta de Docker con los archivos necesarios para empaquetar el servicio en una imagen de Docker:

- `Dockerfile`: especificaciones de imágenes de Docker.
- `build`: crea un contenedor de Docker y almacene la imagen en el registro local.
- `run`: ejecuta el contenedor de Docker desde la imagen almacenada en el registro local.
- `content`: es una carpeta con archivos copiados en la imagen, incluido el script del punto de entrada.

## <a name="azure-iot-hub-setup"></a>Instalación de Azure IoT Hub

Para usar el microservicio, configure Azure IoT Hub para las pruebas de desarrollo e integración.

El proyecto incluye algunos scripts de Bash que le serán de ayuda en esta configuración:

- Crear nuevo IoT Hub: `./scripts/iothub/create-hub.sh`

- Enumerar los centros existentes: `./scripts/iothub/list-hubs.sh`

- Mostrar detalles de IoT Hub (por ejemplo, claves): `./scripts/iothub/show-hub.sh`

En caso de tener varias suscripciones de Azure:

- Mostrar lista de suscripciones: `./scripts/iothub/list-subscriptions.sh`

- Cambiar la suscripción actual: `./scripts/iothub/select-subscription.sh`

## <a name="development-setup"></a>Configuración de desarrollo

### <a name="net-setup"></a>Programa de instalación de .NET

El flujo de trabajo del proyecto se administra a través de [.NET Core](https://dotnet.github.io) 1.x, el cual debe instalar en su entorno, de modo que pueda ejecutar todos los scripts y asegurarse de que su IDE funcione como se espera.

También proporcionamos una [versión de Java](https://github.com/Azure/iot-opc-gds-service-dotnet) de este proyecto y otros componentes de Azure IoT PCS.

### <a name="ide"></a>IDE

Estos son algunos de los IDE que puede usar para trabajar en Azure IoT PCS:

- [Visual Studio](https://www.visualstudio.com)
- [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac)
- [IntelliJ Rider](https://www.jetbrains.com/rider)
- [Visual Studio Code](https://code.visualstudio.com)

### <a name="git-setup"></a>Programa de instalación de Git

El proyecto incluye un enlace de Git para automatizar algunas comprobaciones antes de aceptar un cambio de código. Puede ejecutar las pruebas manualmente o dejar que la plataforma de CI ejecute las mismas. Usaremos el siguiente enlace de Git para ejecutar automáticamente todas las pruebas antes de enviar los cambios de código a GitHub y acelerar el flujo de trabajo de desarrollo.

Si en algún momento quiere eliminar el enlace, solo debe eliminar el archivo instalado en `.git/hooks`. También puede omitir el enlace de confirmación previa mediante la opción `--no-verify`.

#### <a name="pre-commit-hook-with-sandbox"></a>Enlace de confirmación previa con espacio aislado

Para configurar los enlaces incluidos, abra una consola de Windows, Linux o MacOS y ejecute:

```
cd PROJECT-FOLDER
cd scripts/git
setup --with-sandbox
```

Con esta configuración, al insertar archivos en el repositorio, Git comprueba que la aplicación ha pasado todas las pruebas y ejecuta la compilación y las pruebas dentro de un contenedor Docker que está configurado con todos los requisitos de desarrollo.

#### <a name="pre-commit-hook-without-sandbox"></a>Enlace de confirmación previa sin espacio aislado

> [!NOTE] 
> El enlace sin espacio aislado requiere [.NET Core](https://dotnet.github.io) en el sistema PATH.

Para configurar los enlaces incluidos, abra una consola de Windows, Linux o MacOS y ejecute:

```
cd PROJECT-FOLDER
cd scripts/git
setup --no-sandbox
```
Con esta configuración, al insertar archivos en el repositorio, Git comprueba que la aplicación ha pasado todas las pruebas y ejecuta la compilación y las pruebas en su estación de trabajo mediante las herramientas instaladas en el sistema operativo.

Guía sobre el estilo del código del proyecto:

- Dentro de lo razonable, la longitud de las líneas se limita a 80 caracteres como máximo para facilitar el trabajo de las revisiones de códigos y los editores de línea de comandos.

- El código bloquea la sangría con cuatro espacios. Se debe evitar la pestaña char.

- Los archivos de texto usan el formato de fin de línea (LF) de Unix.

- La inserción de dependencias se gestiona con [Autofac](https://autofac.org).

- Los campos de las API del servicio web son de tipo CamelCased excepto los metadatos.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a implementar OPC Vault desde cero, este es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Implementar OPC Twin desde cero](howto-opc-twin-deploy-modules.md)