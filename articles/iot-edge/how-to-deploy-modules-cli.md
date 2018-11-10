---
title: Implementación de módulos de Azure IoT Edge (CLI) | Microsoft Docs
description: Uso de la extensión de IoT de la CLI de Azure para la implementación de módulos en un dispositivo IoT Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 07/27/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: c882321af38b21f4820345b8ce5448227cb121c6
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2018
ms.locfileid: "50977641"
---
# <a name="deploy-azure-iot-edge-modules-with-azure-cli"></a>Implementación de módulos de Azure IoT Edge con la CLI de Azure

Una vez que ha creado módulos de IoT Edge con su lógica empresarial, querrá implementarlos en sus dispositivos para usarlos en el perímetro. Si tiene varios módulos que funcionan conjuntamente para recopilar y procesar datos, puede implementarlos todos a la vez y declarar las reglas de enrutamiento que los conectan. 

La [CLI de Azure](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) es una herramienta de la línea de comandos multiplataforma y de código abierto para la administración de recursos de Azure, como IoT Edge. Esta permite administrar recursos de Azure IoT Hub, instancias de servicio de aprovisionamiento de dispositivos y centros vinculados listos para usar. La extensión de IoT enriquece la CLI de Azure con características como administración de dispositivos y funcionalidad completa de IoT Edge.

En este artículo se muestra cómo crear un manifiesto de implementación de JSON y, luego, usar ese archivo para insertar la implementación en un dispositivo IoT Edge. Para información sobre la creación de una implementación dirigida a varios dispositivos en función de sus etiquetas compartidas, consulte [Implementación y supervisión de módulos de IoT Edge a escala](how-to-deploy-monitor-cli.md).

## <a name="prerequisites"></a>Requisitos previos

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-using-cli.md) en la suscripción de Azure. 
* Un [dispositivo de IoT Edge](how-to-register-device-cli.md) que tenga instalado el entorno de ejecución de Azure IoT Edge.
* La [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) en su entorno. La versión mínima de la CLI de Azure es la 2.0.24. Use `az –-version` para asegurarse. Esta versión admite comandos az extension e introduce la plataforma de comandos de Knack. 
* La [extensión de IoT para la CLI de Azure](https://github.com/Azure/azure-iot-cli-extension).

## <a name="configure-a-deployment-manifest"></a>Configuración de un manifiesto de implementación

Un manifiesto de implementación es un documento JSON que describe qué módulos se van a implementar, cómo fluyen los datos entre los módulos y las propiedades deseadas de los módulos gemelos. Para más información sobre los manifiestos de implementación y cómo crearlos, consulte [Descripción de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).

Para implementar módulos con la CLI de Azure, guarde el manifiesto de implementación localmente como un archivo .json. Usará la ruta de acceso al archivo en la sección siguiente, al ejecutar el comando para aplicar la configuración al dispositivo. 

Este es un manifiesto de implementación básico con un módulo como ejemplo:

   ```json
   {
     "modulesContent": {
       "$edgeAgent": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "runtime": {
             "type": "docker",
             "settings": {
               "minDockerVersion": "v1.25",
               "loggingOptions": "",
               "registryCredentials": {}
             }
           },
           "systemModules": {
             "edgeAgent": {
               "type": "docker",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                 "createOptions": "{}"
               }
             }
           },
           "modules": {
             "tempSensor": {
               "version": "1.0",
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                 "createOptions": "{}"
               }
             }
           }
         }
       },
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {
               "route": "FROM /* INTO $upstream"
           },
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       },
       "tempSensor": {
         "properties.desired": {}
       }
     }
   }
   ```

## <a name="deploy-to-your-device"></a>Implementación en el dispositivo

Para implementar los módulos en el dispositivo, aplique el manifiesto de implementación que configuró con la información del módulo. 

Cambie los directorios a la carpeta donde se guarda el manifiesto de implementación. Si ha usado una de las plantillas de IoT Edge para VS Code, use el archivo `deployment.json` de la carpeta **config** del directorio de la solución. No use el archivo `deployment.template.json`. 

Utilice el siguiente comando para aplicar la configuración a un dispositivo IoT Edge:

   ```cli
   az iot edge set-modules --device-id [device id] --hub-name [hub name] --content [file path]
   ```

El parámetro "device id" distingue mayúsculas y minúsculas. El parámetro "content" apunta al archivo del manifiesto de implementación que guardó. 

   ![Establecimiento de los módulos](./media/how-to-deploy-cli/set-modules.png)

## <a name="view-modules-on-your-device"></a>Visualización de módulos en el dispositivo

Una vez que los módulos se han implementado en el dispositivo, puede verlos todos con el siguiente comando: 

Vea los módulos en el dispositivo de IoT Edge:
    
   ```cli
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

El parámetro "device id" distingue mayúsculas y minúsculas.

   ![Enumere los módulos](./media/how-to-deploy-cli/list-modules.png)

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [implementar y supervisar módulos de IoT Edge a escala](how-to-deploy-monitor.md)
