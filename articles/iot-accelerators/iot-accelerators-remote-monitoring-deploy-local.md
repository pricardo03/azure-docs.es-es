---
title: 'Implementación local de la solución de supervisión remota: Azure | Microsoft Docs'
description: En esta guía paso a paso se muestra cómo implementar el acelerador de soluciones de supervisión remota en la máquina local para pruebas y desarrollo.
author: asdonald
manager: timlt
ms.author: asdonald
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/26/2018
ms.topic: conceptual
ms.openlocfilehash: 7007b1406dbcfab3af4700418ac2ce07b9e521c0
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47407440"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally"></a>Implementación local del acelerador de la solución Supervisión remota

En este artículo, se explica cómo implementar el acelerador de la solución Supervisión remota en la máquina local para pruebas y desarrollo. En el enfoque descrito en este artículo se implementan los microservicios en un contenedor de Docker local y se usan los servicios IoT Hub, Cosmos DB y Azure Time Series Insights en la nube. Para aprender a ejecutar el acelerador de soluciones de supervisión remota en un IDE en la máquina local, consulte [Starting Microservices on local environment](https://github.com/Azure/remote-monitoring-services-java/blob/master/docs/LOCAL_DEPLOYMENT.md) (Inicio de microservicios en el entorno local) en GitHub.

## <a name="prerequisites"></a>Requisitos previos

Para implementar los servicios de Azure que usa el acelerador de la solución Supervisión remota, necesita una suscripción de Azure activa.

En caso de no tener ninguna, puede crear una cuenta de evaluación gratuita en tan solo unos minutos. Para obtener más información, consulte [Evaluación gratuita de Azure](http://azure.microsoft.com/pricing/free-trial/).

Para completar la implementación local, necesita que las herramientas siguientes estén instaladas en la máquina de desarrollo local:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Docker Compose](https://docs.docker.com/compose/install/)
* [Node.js v8](https://nodejs.org/): este software es un requisito previo para la CLI de PCS que usan los scripts para crear recursos de Azure. No use Node.js v10.

> [!NOTE]
> Estas herramientas están disponibles en varias plataformas, como Windows, Linux e iOS.

### <a name="download-the-source-code"></a>Descarga del código fuente

El repositorio de GitHub de código fuente de supervisión remota incluye los archivos de configuración de Docker que necesita para descargar, configurar y ejecutar las imágenes de Docker que contienen los microservicios. Para clonar y crear una versión local del repositorio, use el entorno de línea de comandos para ir a una carpeta adecuada en la máquina local y, luego, ejecute uno de los siguientes comandos:

Para descargar la versión más reciente de las implementaciones de microservicios de Java, ejecute:

```cmd/sh
git clone https://github.com/Azure/remote-monitoring-services-java.git
```

Para descargar la versión más reciente de las implementaciones de microservicios de .NET, ejecute:

```cmd\sh
git clone https://github.com/Azure/remote-monitoring-services-dotnet.git
```

> [!NOTE]
> Estos comandos descargan el código fuente de todos los microservicios, además de los scripts que se usan para ejecutar los microservicios localmente. Aunque no necesita el código fuente para ejecutar los microservicios en Docker, el código fuente es útil si más tarde piensa modificar el acelerador de soluciones y probar los cambios localmente.

## <a name="deploy-the-azure-services"></a>Implementación de servicios de Azure

Aunque en este artículo se muestra cómo ejecutar los microservicios localmente, dependen de los servicios de Azure que se ejecutan en la nube. Puede implementar estos servicios de Azure de manera [manual mediante Azure Portal](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Manual-steps-to-create-azure-resources-for-local-setup), o usar el script proporcionado. En los siguientes ejemplos de scripts se da por hecho que usa el repositorio de .NET en una máquina Windows. Si está trabajando en otro entorno, ajuste las rutas de acceso, las extensiones de archivo y los separadores de ruta de acceso de la manera correcta. Puede usar los scripts proporcionados para:

### <a name="create-new-azure-resources"></a>Crear nuevos recursos de Azure

Si aún no ha creado los recursos de Azure necesarios, siga estos pasos:

1. En el entorno de línea de comandos, vaya a la carpeta **remote-monitoring-services-dotnet\scripts\local\launch** de la copia clonada del repositorio.

2. Ejecute el script **start.cmd** y siga los avisos. El script le pide que inicie sesión en su cuenta de Azure y que reinicie el script. El script le pide entonces la siguiente información:
    * Nombre de la solución.
    * La suscripción de Azure que se va a usar.
    * La ubicación del centro de datos de Azure que se va a usar.

    El script crea el grupo de recursos en Azure con el nombre de la solución. Este grupo de recursos contiene los recursos de Azure que usa el acelerador de soluciones.

3. Cuando finaliza el script, se muestra una lista de variables de entorno. Siga las instrucciones de la salida del comando para guardar estas variables en el archivo **remote-monitoring-services-dotnet\\scripts\\local\\.env**.

### <a name="use-existing-azure-resources"></a>Usar recursos de Azure existentes

Si ya ha creado los recursos de Azure necesarios, edite las definiciones de variable de entorno en el archivo **remote-monitoring-services-dotnet\\scripts\\local\\.env** con los valores requeridos. El archivo **.env** contiene información detallada sobre dónde encontrar los valores necesarios.

## <a name="run-the-microservices-in-docker"></a>Ejecución de los microservicios en Docker

Los microservicios que se ejecutan en los contenedores de Docker locales necesitan acceso a los servicios que se ejecutan en Azure. Puede probar la conectividad a Internet de su entorno de Docker mediante el siguiente comando que inicia un contenedor pequeño e intenta hacer ping a una dirección de Internet:

```cmd/sh
docker run --rm -ti library/alpine ping google.com
```

Para ejecutar el acelerador de soluciones, vaya a la carpeta **remote-monitoring-services-dotnet\\scripts\\local** en el entorno de la línea de comandos y ejecute el siguiente comando:

```cmd\sh
docker-compose up
```

La primera vez que ejecuta este comando, Docker descarga las imágenes de microservicios desde Docker Hub para crear los contenedores localmente. En las ejecuciones posteriores, Docker ejecuta los contenedores inmediatamente.

Puede utilizar un shell independiente para ver los registros desde el contenedor. En primer lugar, busque el identificador del contenedor mediante el comando `docker ps -a`. A continuación, utilice `docker logs {container-id} --tail 1000` para ver las últimas 1000 entradas de registro en el contenedor especificado.

Para acceder al panel de la solución Supervisión remota, vaya a [http://localhost:8080](http://localhost:8080) en el explorador.

## <a name="clean-up"></a>Limpieza

Para evitar cargos innecesarios, cuando haya terminado las pruebas, elimine los servicios en la nube de su suscripción de Azure. La forma más sencilla de quitar los servicios es ir a [Azure Portal](https://ms.portal.azure.com) y eliminar el grupo de recursos que se creó cuando ejecutó el script **start.cmd**.

Utilice el comando `docker-compose down --rmi all` para quitar las imágenes de Docker y liberar espacio en la máquina local. También puede eliminar la copia local del repositorio de Supervisión remota que se ha creado al clonar el código fuente de GitHub.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Configuración del entorno de desarrollo local
> * Configuración del acelerador de la solución
> * Implementación del acelerador de solución
> * Inicio de sesión en el acelerador de la solución

Ahora que ha implementado la solución Supervisión remota, el paso siguiente es [explorar las funcionalidades del panel de soluciones](quickstart-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->
