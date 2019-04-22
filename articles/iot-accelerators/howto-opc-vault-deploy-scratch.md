---
title: Cómo implementar el módulo de OPC almacén desde el principio - Azure | Microsoft Docs
description: Cómo implementar OPC almacén desde el principio.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: dedba42d7e4b8d603d97522cf0173f41efd20b3a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59496903"
---
# <a name="deploy-opc-vault-from-scratch"></a>Implementación de OPC Vault desde cero

Almacén de OPC es un microservicio que puede configurar, registrar y administrar el ciclo de vida de certificado de servidor OPC UA y las aplicaciones cliente en la nube. Este artículo muestra cómo implementar OPC almacén desde el principio.

## <a name="configuration-and-environment-variables"></a>Variables de entorno y configuración

La configuración del servicio se almacena utilizando adaptadores de configuración de ASP.NET Core, en appsettings.ini. El formato INI permite almacenar los valores en un formato legible, con comentarios.
La aplicación también admite la inserción variables de entorno, como las credenciales y los detalles de redes. (Esta sección originalmente se titula variables de entorno y configuración de la lista de tareas).

El archivo de configuración en el repositorio, se hace referencia a algunas variables de entorno que necesitan al menos una vez creado. Dependiendo de su sistema operativo y el IDE, hay varias maneras de administrar variables de entorno:

- Para los usuarios de Windows, el script setup.cmd de env-var debe se prepara y ejecuta una sola vez. Cuando se ejecuta, se conservará la configuración a través de reinicios y sesiones de terminal Server.

- En entornos Linux y OSX, se debe ejecutar cada vez que se abre una nueva consola de la secuencia de comandos env-var-setup. Según el sistema operativo y el terminal, hay métodos para conservar valores globalmente, para obtener más información que deberían ayudar estas páginas:

  - https://stackoverflow.com/questions/13046624/how-to-permanently-export-a-variable-in-linux
  
  - https://stackoverflow.com/questions/135688/setting-environment-variables-in-os-x
  
  - https://help.ubuntu.com/community/EnvironmentVariables
  

- Visual Studio: Las variables de entorno se puede establecer también desde Visual Studio, las propiedades del proyecto, en el panel izquierdo, seleccione "Propiedades de configuración" y "Entorno" para llegar a una sección donde puede agregar varias variables.

- IntelliJ Rider: Se pueden establecer variables de entorno en cada configuración de ejecución, de forma similar a IntelliJ IDEA https://www.jetbrains.com/help/idea/run-debug-configuration-application.html

## <a name="run-and-debug-with-visual-studio"></a>Ejecutar y depurar con Visual Studio

Visual Studio le permite abrir rápidamente la aplicación sin usar un símbolo del sistema, sin configurar nada fuera del IDE.

Pasos con Visual Studio 2017:

1. Abra la solución mediante el `iot-opc-gds-service.sln` archivo.

1. Cuando se carga la solución, haga doble clic en el `WebService` del proyecto, seleccione y vaya a la `Debug` sección.

1. En la misma sección, defina las variables de entorno necesarias.

1. Presione **F5**, o el **ejecutar** icono. Visual Studio debe abrir el explorador que muestra el estado del servicio en formato JSON.

## <a name="run-and-debug-with-intellij-rider"></a>Ejecutar y depurar con IntelliJ Rider

1. Abra la solución mediante el `iot-opc-gds-service.sln` archivo.

1. Cuando se carga la solución, vaya a `Run > Edit Configurations` y cree un nuevo `.NET Project` configuración.

1. En la configuración, seleccione el proyecto de servicio Web.

1. Guardar la configuración y ejecute la configuración que se crea a partir de la barra de herramientas IDE.

1. Debería ver que el servicio de mensajes en la ventana de la ejecución de IntelliJ con detalles como la dirección URL donde se ejecuta el servicio web y el servicio de arranque los registros.

## <a name="build-and-run-from-the-command-line"></a>Compilar y ejecutar desde la línea de comandos

La carpeta scripts contiene algunos scripts para las tareas frecuentes:

- `build`: Todos los proyectos de compilar y ejecutar las pruebas.

- `compile`: Compilar todos los proyectos.

- `run`: Compile los proyectos y ejecute el servicio, que solicita privilegios elevados en Windows para ejecutar el servicio web.

Comprobación las secuencias de comandos para la configuración de las variables de entorno. Puede establecer las variables de entorno global en su sistema operativo, o usar la secuencia de comandos "env-var-setup" en la carpeta scripts.

### <a name="sandbox"></a>Espacio aislado

Las secuencias de comandos suponen que ha configurado el entorno de desarrollo con .NET Core y Docker. Puede evitar la instalación de .NET Core e instalar Docker solo y utilice el parámetro de línea de comandos `--in-sandbox` (o la forma abreviada `-s`), por ejemplo:

- `build --in-sandbox`: Ejecuta la tarea de compilación dentro de un contenedor de Docker (forma abreviada `build -s`).

- `compile --in-sandbox`: Ejecuta la tarea de compilación dentro de un contenedor de Docker (forma abreviada `compile -s`).

- `run --in-sandbox`: Inicia el servicio dentro de un contenedor de Docker (forma abreviada `run -s`).

Las imágenes de Docker que se usan para el espacio aislado se hospedan en Docker Hub [aquí](https://hub.docker.com/r/azureiotpcs/code-builder-dotnet).

## <a name="package-the-application-to-a-docker-image"></a>Empaquetar la aplicación a una imagen de Docker

El `scripts` carpeta incluye una subcarpeta de docker con los archivos necesarios para empaquetar el servicio en una imagen de Docker:

- `Dockerfile`: Especificaciones de las imágenes de docker.
- `build`: Crear un contenedor de Docker y almacenar la imagen en el registro local.
- `run`: Ejecute el contenedor de Docker desde la imagen almacenada en el registro local.
- `content`: Una carpeta con los archivos copiados en la imagen, incluido el script de punto de entrada.

## <a name="azure-iot-hub-setup"></a>El programa de instalación de Azure IoT Hub

Para usar el microservicio, configure su centro de IoT de Azure para las pruebas de integración y desarrollo.

El proyecto incluye algunos scripts de Bash para ayudarle con este programa de instalación:

- Crear nuevo centro de IoT: `./scripts/iothub/create-hub.sh`

- Lista de los centros de existentes: `./scripts/iothub/list-hubs.sh`

- Mostrar detalles (por ejemplo, las claves) de IoT Hub: `./scripts/iothub/show-hub.sh`

Y, en caso de que había varias suscripciones de Azure:

- Mostrar lista de suscripciones: `./scripts/iothub/list-subscriptions.sh`

- Cambiar la suscripción actual: `./scripts/iothub/select-subscription.sh`

## <a name="development-setup"></a>Configuración de desarrollo

### <a name="net-setup"></a>Programa de instalación de .NET

El flujo de trabajo del proyecto se administra a través de [.NET Core](https://dotnet.github.io) 1.x, que debe instalar en su entorno, por lo que puede ejecutar todos los scripts y asegurarse de que el IDE funciona según lo previsto.

También proporcionamos un [versión Java](https://github.com/Azure/iot-opc-gds-service-dotnet) de este proyecto y otros componentes de Azure IoT PCS.

### <a name="ide"></a>IDE

Estos son algunos de los IDE que puede usar para trabajar en los equipos de IoT de Azure:

- [Visual Studio](https://www.visualstudio.com)
- [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac)
- [IntelliJ Rider](https://www.jetbrains.com/rider)
- [Visual Studio Code](https://code.visualstudio.com)

### <a name="git-setup"></a>Programa de instalación de GIT

El proyecto incluye un enlace de Git, para automatizar algunas comprobaciones antes de aceptar un cambio de código. Puede ejecutar las pruebas manualmente o dejar que la plataforma de integración continua para ejecutar las pruebas. Usamos el enlace de Git siguiente para ejecutar todas las pruebas antes de enviar los cambios de código en GitHub automáticamente y acelerar el flujo de trabajo de desarrollo.

Si desea quitar el enlace en cualquier momento, basta con eliminar el archivo instalado en `.git/hooks`. También puede omitir el uso del enlace previas a la confirmación del `--no-verify` opción.

#### <a name="pre-commit-hook-with-sandbox"></a>Enlace previas a la confirmación con recinto de seguridad

Para configurar los enlaces incluidos, abra una consola de Windows, Linux y MacOS y ejecute:

```
cd PROJECT-FOLDER
cd scripts/git
setup --with-sandbox
```

Con esta configuración, cuando se protejan los archivos, Git comprueba que la aplicación pasa todas las pruebas, ejecutar la compilación y las pruebas dentro de un contenedor de Docker que se configura con todos los requisitos de desarrollo.

#### <a name="pre-commit-hook-without-sandbox"></a>Enlace previas a la confirmación sin espacio aislado

> [!NOTE] 
> Requiere el enlace sin espacio aislado [.NET Core](https://dotnet.github.io) en la ruta de acceso del sistema.

Para configurar los enlaces incluidos, abra una consola de Windows, Linux y MacOS y ejecute:

```
cd PROJECT-FOLDER
cd scripts/git
setup --no-sandbox
```
Con esta configuración, cuando se protejan los archivos, Git comprueba que la aplicación pasa todas las pruebas, ejecutar la compilación y las pruebas en su estación de trabajo mediante las herramientas instaladas en su sistema operativo.

Orientación sobre el estilo de código del proyecto:

- Dentro de lo razonable, la longitud de las líneas se limita a 80 caracteres como máximo para ayudar a las revisiones de código y editores de línea de comandos.

- Sangría de bloques de código con cuatro espacios. Debe evitarse el carácter de la pestaña.

- Archivos de texto usan final de Unix del formato de línea (LF).

- Inserción de dependencias se administra con [Autofac](https://autofac.org).

- Los campos de las API de servicio Web son CamelCased excepto los metadatos.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido cómo implementar OPC almacén desde el principio, aquí es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Implementar a OPC gemelo desde cero](howto-opc-twin-deploy-modules.md)