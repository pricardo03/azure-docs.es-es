---
title: 'Creación de implementaciones automáticas desde la línea de comandos: Azure IoT Edge | Microsoft Docs'
description: Uso de la extensión de IoT para la CLI de Azure para crear implementaciones automáticas para grupos de dispositivos IoT Edge
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 07/25/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 64c4b82208b2f8a20f7fd00fb574d5e017030e81
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53094158"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-cli"></a>Implementar y supervisar módulos de IoT Edge a escala, mediante la CLI de Azure

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-edge-how-to-deploy-monitor-selector.md)]

Azure IoT Edge permite mover los análisis al perímetro y proporciona una interfaz en la nube para que pueda administrar y supervisar los dispositivos IoT Edge de forma remota. La capacidad de administrar dispositivos de forma remota es cada vez más importante, ya que las soluciones de Internet de las cosas son cada vez más grandes y complejas. Azure IoT Edge está pensado para apoyar sus objetivos empresariales, sin importar cuántos dispositivos agregue.

Puede administrar dispositivos individuales e implementarles módulos de uno en uno. Sin embargo, si desea realizar cambios en los dispositivos en a gran escala, puede crear una **implementación automática de IoT Edge**, que forma parte de la administración automática de dispositivos de IoT Hub. Las implementaciones son procesos dinámicos que permiten poner en marcha varios módulos en múltiples dispositivos a la vez, realizar un seguimiento del estado y del mantenimiento de los módulos y realizar cambios cuando sea necesario. 

En este artículo, configuró la CLI de Azure y la extensión de IoT. A continuación, aprenderá a implementar módulos en un dispositivo IoT Edge y a supervisar el progreso mediante los comandos de la CLI disponibles.

## <a name="cli-prerequisites"></a>Requisitos previos de CLI

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-using-cli.md) en la suscripción de Azure. 
* [Dispositivos de IoT Edge](how-to-register-device-cli.md) que tengan instalado el entorno de ejecución de IoT Edge.
* La [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) en su entorno. La versión mínima de la CLI de Azure es la 2.0.24. Use `az –-version` para asegurarse. Esta versión admite comandos az extension e introduce la plataforma de comandos de Knack. 
* La [extensión de IoT para la CLI de Azure](https://github.com/Azure/azure-iot-cli-extension).

## <a name="configure-a-deployment-manifest"></a>Configuración de un manifiesto de implementación

Un manifiesto de implementación es un documento JSON que describe qué módulos se van a implementar, cómo fluyen los datos entre los módulos y las propiedades deseadas de los módulos gemelos. Para más información sobre los manifiestos de implementación y cómo crearlos, consulte [Descripción de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).

Para implementar módulos mediante la CLI de Azure, guarde localmente el manifiesto de implementación como un archivo .txt. Usará la ruta de acceso al archivo en la sección siguiente, al ejecutar el comando para aplicar la configuración al dispositivo. 

Este es un manifiesto de implementación básico con un módulo como ejemplo:

   ```json
   {
        "content": {
         "modulesContent": {
           "$edgeAgent": {
             "properties.desired": {
               "schemaVersion": "1.0",
               "runtime": {
                 "type": "docker",
                 "settings": {
                   "minDockerVersion": "v1.25",
                   "loggingOptions": "",
                   "registryCredentials": {
                     "registryName": {
                       "username": "",
                       "password": "",
                       "address": ""
                     }
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
   }
   ```


## <a name="identify-devices-using-tags"></a>Identificación de dispositivos mediante etiquetas

Antes de crear una implementación, tendrá que especificar los dispositivos que desea afectar. Azure IoT Edge identifica los dispositivos mediante **etiquetas** en el dispositivo gemelo. Cada dispositivo puede tener varias etiquetas y puede definirlas de cualquier manera que tenga sentido para su solución. Por ejemplo, si administra un campus de edificios inteligentes, puede agregar las siguientes etiquetas a un dispositivo:

```json
"tags":{
    "location":{
        "building": "20",
        "floor": "2"
    },
    "roomtype": "conference",
    "environment": "prod"
}
```

Para más información sobre dispositivos gemelos y etiquetas, consulte [Información y uso de dispositivos gemelos en IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md).

## <a name="create-a-deployment"></a>de una implementación

Implemente módulos en los dispositivos de destino; para ello, cree una implementación que consista en el manifiesto de implementación y otros parámetros. 

Use los comandos siguientes para crear una implementación:

   ```cli
   az iot edge deployment create --deployment-id [deployment id] --labels [labels] --content [file path] --hub-name [hub name] --target-condition [target query] --priority [int]
   ```

* **--deployment-id**: el nombre de la implementación que se creará en IoT Hub. Asigne a su implementación un nombre exclusivo de hasta 128 letras en minúscula. Evite los espacios y los siguientes caracteres no válidos: `& ^ [ ] { } \ | " < > /`.
* **--labels**: agregue etiquetas para realizar un seguimiento de las implementaciones. Las etiquetas son pares de Nombre y Valor que describen la implementación. Por ejemplo, `HostPlatform, Linux` o `Version, 3.0.1`.
* **--content** -ruta del archivo del manifiesto de implementación JSON. 
* **--hub-name**: nombre de la instancia de IoT Hub en la que se creará la implementación. El centro debe estar en la suscripción actual. Cambie a la suscripción que quiera usar con el comando `az account set -s [subscription name]`.
* **--target-condition**: escriba una condición de destino para determinar qué dispositivos se dirigirán a esta implementación. La condición se basa en las etiquetas del dispositivo gemelo o en las propiedades notificadas del dispositivo gemelo y debe coincidir con el formato de expresión. Por ejemplo, `tags.environment='test'` o `properties.reported.devicemodel='4000x'`. 
* **--priority**: debe ser un entero positivo. En el caso de que dos o más implementaciones se destinen al mismo dispositivo, se aplicará la implementación con el valor numérico más alto para la prioridad.

## <a name="monitor-a-deployment"></a>Supervisión de una implementación

Use los comandos siguientes para ver el contenido de una implementación:

   ```cli
az iot edge deployment show --deployment-id [deployment id] --hub-name [hub name]
   ```
* **--deployment-id**: el nombre de la implementación que está en IoT Hub.
* **--hub-name**: nombre de la instancia de IoT Hub en la que está la implementación. El centro debe estar en la suscripción actual. Cambie a la suscripción que quiera usar con el comando `az account set -s [subscription name]`.

Inspeccione la implementación en la ventana de comandos. La propiedad **metrics** enumera un recuento para cada métrica que evalúa cada centro:
* **targetedCount**: es una métrica del sistema que especifica la cantidad de dispositivos gemelos que hay en IoT Hub que coinciden con las condiciones del destino.
* **appliedCount**: es una métrica del sistema que especifica la cantidad de dispositivos cuyo contenido de implementación se ha aplicado a sus módulos gemelos en IoT Hub.
* **reportedSuccessfulCount**: es una métrica de dispositivo que especifica la cantidad de dispositivos de Edge en el informe de implementación correcto, a partir del tiempo de ejecución del cliente de IoT Edge.
* **reportedFailedCount**: es una métrica de dispositivo que especifica la cantidad de dispositivos de Edge en el informe de implementación con errores, a partir del tiempo de ejecución del cliente de IoT Edge.

Puede mostrar una lista de identificadores u objetos de dispositivos para cada una de las métricas mediante el siguiente comando:

   ```cli
az iot edge deployment show-metric --deployment-id [deployment id] --metric-id [metric id] --hub-name [hub name] 
   ```

* **--deployment-id**: el nombre de la implementación que está en IoT Hub.
* **--metric-id**: el nombre de la métrica de la cual quiere ver la lista de identificadores de dispositivos como, por ejemplo, `reportedFailedCount`.
* **--hub-name**: nombre de la instancia de IoT Hub en la que está la implementación. El centro debe estar en la suscripción actual. Cambie a la suscripción que quiera usar con el comando `az account set -s [subscription name]`.

## <a name="modify-a-deployment"></a>Modificación de una implementación

Cuando se modifica una implementación, los cambios se replican inmediatamente a todos los dispositivos seleccionados. 

Si actualiza la condición de destino, se producen las siguientes actualizaciones:
* Si un dispositivo no cumplía la antigua condición de destino, pero cumple la nueva condición de destino y esta implementación es la prioridad más alta para ese dispositivo, esta implementación se aplica al dispositivo. 
* Si un dispositivo que actualmente ejecuta esta implementación ya no cumple la condición de destino, desinstala esta implementación y asume la siguiente implementación de mayor prioridad. 
* Si un dispositivo que actualmente ejecuta esta implementación ya no cumple la condición de destino y no cumple la condición de destino de cualquier otra implementación, no se produce ningún cambio en el dispositivo. El dispositivo sigue ejecutando los módulos actuales en su estado actual, pero ya no se administra como parte de esta implementación. Cuando se cumple la condición de destino de cualquier otra implementación, desinstala esta implementación y adopta una nueva. 

Use los comandos siguientes para actualizar una implementación:

   ```cli
az iot edge deployment update --deployment-id [deployment id] --hub-name [hub name] --set [property1.property2='value']
   ```
* **--deployment-id**: el nombre de la implementación que está en IoT Hub.
* **--hub-name**: nombre de la instancia de IoT Hub en la que está la implementación. El centro debe estar en la suscripción actual. Cambie a la suscripción que quiera usar con el comando `az account set -s [subscription name]`.
* **--set**: actualizar una propiedad de la implementación. Puede actualizar las propiedades siguientes:
    * targetCondition: por ejemplo, `targetCondition=tags.location.state='Oregon'`.
    * labels 
    * prioridad


## <a name="delete-a-deployment"></a>Eliminación de una implementación

Cuando se elimina una implementación, los dispositivos adoptan la siguiente implementación de prioridad más alta. Si los dispositivos no cumplen la condición de destino de alguna implementación, los módulos no se quitan cuando se elimina la implementación. 

Use los comandos siguientes para eliminar una implementación:

   ```cli
az iot edge deployment delete --deployment-id [deployment id] --hub-name [hub name] 
   ```
* **--deployment-id**: el nombre de la implementación que está en IoT Hub.
* **--hub-name**: nombre de la instancia de IoT Hub en la que está la implementación. El centro debe estar en la suscripción actual. Cambie a la suscripción que quiera usar con el comando `az account set -s [subscription name]`.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo [implementar módulos en dispositivos Edge](module-deployment-monitoring.md).
