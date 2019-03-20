---
title: archivo de inclusión
description: archivo de inclusión
services: iot-accelerators
author: v-krghan
ms.service: iot-accelerators
ms.topic: include
ms.date: 01/25/2019
ms.author: v-krghan
ms.custom: include file
ms.openlocfilehash: 81590a4d686d85482bee38c4391b8ac24b25658e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58114065"
---
## <a name="download-the-source-code"></a>Descarga del código fuente

Los repositorios de código fuente de Supervisión remota incluyen el código fuente y los archivos de configuración de Docker que necesita para ejecutar las imágenes de Docker de los microservicios.

Para clonar y crear una versión local del repositorio, use el entorno de línea de comandos para ir a una carpeta adecuada en la máquina local. A continuación, ejecute uno de los siguientes conjuntos de comandos para clonar el repositorio de Java:

Para descargar la versión más reciente de las implementaciones de microservicios de Java, ejecute:


```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-java
git submodule foreach git pull origin master
```

> [!NOTE]
> Estos comandos descargan el código fuente de todos los microservicios, además de los scripts que se usan para ejecutar los microservicios localmente. Aunque no necesita el código fuente para ejecutar los microservicios en Docker, el código fuente es útil si más tarde piensa modificar el acelerador de soluciones y probar los cambios localmente.

## <a name="deploy-the-azure-services"></a>Implementación de servicios de Azure

Aunque en este artículo se muestra cómo ejecutar los microservicios localmente, dependen de los servicios de Azure que se ejecutan en la nube. Use el siguiente script para implementar los servicios de Azure. En los siguientes scripts de ejemplo se da por hecho que usa el repositorio de Java en una máquina Windows. Si está trabajando en otro entorno, ajuste las rutas de acceso, las extensiones de archivo y los separadores de ruta de acceso de la manera correcta.

### <a name="create-new-azure-resources"></a>Crear nuevos recursos de Azure

Si aún no ha creado los recursos de Azure necesarios, siga estos pasos:

1. En el entorno de línea de comandos, vaya a la carpeta **\services\scripts\local\launch** de la copia clonada del repositorio.

1. Ejecute los comandos siguientes para instalar la herramienta de la CLI **pcs** e inicie sesión en su cuenta de Azure:

    ```cmd
    npm install -g iot-solutions
    pcs login
    ```

1. Ejecute el script **start.cmd**. El script le pedirá la siguiente información:
   * Nombre de la solución.
   * La suscripción de Azure que se va a usar.
   * La ubicación del centro de datos de Azure que se va a usar.

     El script crea el grupo de recursos en Azure con el nombre de la solución. Este grupo de recursos contiene los recursos de Azure que usa el acelerador de soluciones. Puede eliminar este grupo de recursos cuando ya no necesite los recursos correspondientes.

     El script también agrega un conjunto de variables de entorno con un prefijo **PCS** en el equipo local. Al iniciar los contenedores de Docker o los proyectos de microservicios localmente, leen los valores de configuración de estas variables de entorno.

     > [!TIP]
     > Cuando se completa el script, también guarda las variables de entorno en un archivo denominado  **\<la carpeta principal\>\\.pcs\\\<nombre de la solución\>.env**. Puede usarlos para las implementaciones del acelerador de soluciones futuras. Tenga en cuenta que cualquier variable de entorno establecida en el equipo local invalida los valores del archivo **services\\scripts\\local\\.env** al ejecutar **docker-compose**.

1. Salga del entorno de la línea de comandos.

### <a name="use-existing-azure-resources"></a>Usar recursos de Azure existentes

Si ya ha creado los recursos de Azure necesarios, cree las variables de entorno correspondientes en el equipo local. Pueden guardarse en el archivo  **\<carpeta principal\>\\.pcs\\\<nombre de la solución\>.env** de la implementación. Tenga en cuenta que las variables de entorno establecidas en el equipo local invalidan los valores del archivo **services\\scripts\\local\\.env** al ejecutar **docker-compose**.