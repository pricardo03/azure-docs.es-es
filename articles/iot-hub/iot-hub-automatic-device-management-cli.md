---
title: Administración automática de dispositivos a escala con el centro de IoT de Azure (CLI) | Microsoft Docs
description: Utilice Administración automática de dispositivos de Azure IoT Hub para asignar una configuración a varios dispositivos de IoT
author: ChrisGMsft
manager: bruz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: chrisgre
ms.openlocfilehash: 0302146634904ccf1d87220d3a24553149e10372
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2019
ms.locfileid: "59012971"
---
# <a name="automatic-iot-device-management-at-scale-using-the-azure-cli"></a>Administración automática de dispositivos IoT a escala mediante la CLI de Azure

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-hub-auto-device-config-selector.md)]

Administración automática de dispositivos en Azure IoT Hub automatiza muchas de las tareas repetitivas y complejas de administración de flotas de dispositivo de gran tamaño. Administración automática de dispositivos, puede tener como destino un conjunto de dispositivos basándose en sus propiedades, definir una configuración deseada y, a continuación, permitir que IoT Hub actualice los dispositivos cuando estén dentro del ámbito. Esta actualización se realiza mediante un _configuración automática de dispositivos_, lo que permite resumir la finalización y cumplimiento, controlar combinaciones y conflictos y revertir las configuraciones en un enfoque por fases.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Funciona de la administración automática de dispositivos mediante la actualización de un conjunto de dispositivos gemelos con las propiedades deseadas y un resumen de la que se basa en el dispositivo gemelo reporting propiedades notificadas.  Introduce una nueva clase y el documento JSON que llama a un *configuración* que tiene tres partes:

* La **condición de destino** define el ámbito de los dispositivos gemelos que se van a actualizar. La condición de destino se especifica como una consulta de las propiedades notificadas o de las etiquetas de los dispositivos gemelos.

* El **contenido de destino** define las propiedades deseadas que se van a agregar o actualizar en los dispositivos gemelos de destino. El contenido incluye una ruta de acceso a la sección de las propiedades que desea cambiar.

* Las **métricas** definen el número de resúmenes de los distintos estados de configuración, como **Correcto**, **En curso** y **Error**. Las métricas personalizadas se especifican como consultas de las propiedades notificadas de los dispositivos gemelos.  Las métricas del sistema son las métricas predeterminadas que miden el estado de actualización gemelos, como el número de dispositivos gemelos que tienen como destino y el número de dispositivos gemelos que se han actualizado correctamente. 

## <a name="cli-prerequisites"></a>Requisitos previos de la CLI

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-using-cli.md) en la suscripción de Azure. 
* La [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) en su entorno. La versión mínima de la CLI de Azure es la 2.0.24. Use `az –-version` para asegurarse. Esta versión admite comandos az extension e introduce la plataforma de comandos de Knack. 
* La [extensión de IoT para la CLI de Azure](https://github.com/Azure/azure-iot-cli-extension).

## <a name="implement-device-twins-to-configure-devices"></a>Implementación de dispositivos gemelos para configurar dispositivos

Las configuraciones automáticas de dispositivos requieren el uso de dispositivos gemelos para sincronizar el estado entre la nube y los dispositivos.  Consulte [Información y uso de los dispositivos gemelos en IoT Hub](iot-hub-devguide-device-twins.md) para obtener orientación sobre el uso de los dispositivos gemelos.

## <a name="identify-devices-using-tags"></a>Identificación de dispositivos mediante etiquetas

Antes de crear una configuración, debe especificar qué dispositivos desea afectar. Azure IoT Hub identifica los dispositivos mediante etiquetas en el dispositivo gemelo. Cada dispositivo puede tener varias etiquetas y puede definirlas de cualquier manera que tenga sentido para su solución. Por ejemplo, si administra dispositivos en distintas ubicaciones, agregue las siguientes etiquetas a un dispositivo gemelo:

```json
"tags": {
    "location": {
        "state": "Washington",
        "city": "Tacoma"
    }
},
```

## <a name="define-the-target-content-and-metrics"></a>Definir el contenido de destino y las métricas

El contenido de destino y métrica de las consultas se especifican como documentos JSON que describen el dispositivo gemelo propiedades deseadas para el conjunto y las propiedades notificadas para medir.  Para crear una configuración automática de dispositivos mediante la CLI de Azure, guarde el contenido y las métricas de destino de forma local como archivos .txt. Utilice las rutas de acceso de archivo en una sección posterior al ejecutar el comando para aplicar la configuración en el dispositivo. 

Aquí tiene un ejemplo de contenido de destino básico:

```json
{
  "content": {
    "deviceContent": {
      "properties.desired.chillerWaterSettings": {
        "temperature": 38,
        "pressure": 78
      }
    }
}
```

Aquí tiene ejemplos de consultas de métrica:

```json
{
  "queries": {
    "Compliant": "select deviceId from devices where configurations.[[chillersettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='current'",
    "Error": "select deviceId from devices where configurations.[[chillersettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='error'",
    "Pending": "select deviceId from devices where configurations.[[chillersettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='pending'"
  }
}
```

## <a name="create-a-configuration"></a>Creación de una configuración

Debe configurar los dispositivos de destino creando una configuración que tenga el contenido y las métricas de destino. 

Use el comando siguiente para crear una configuración:

```cli
   az iot hub configuration create --config-id [configuration id] \
     --labels [labels] --content [file path] --hub-name [hub name] \
     --target-condition [target query] --priority [int] \
     --metrics [metric queries]
```

* --**config-id**: el nombre de la configuración que se creará en IoT Hub. Asigne a su configuración un nombre exclusivo de hasta 128 letras en minúscula. Evite los espacios y los siguientes caracteres no válidos: `& ^ [ ] { } \ | " < > /`.

* --**labels**: agrega etiquetas para ayudar a realizar un mejor seguimiento de la configuración. Las etiquetas son pares de Nombre y Valor que describen la implementación. Por ejemplo, `HostPlatform, Linux` o `Version, 3.0.1`

* --**content**: formato JSON alineado o ruta de acceso del archivo al contenido de destino que se establecerá como las propiedades gemelas que quiera. 

* --**hub-name**: nombre de la instancia de IoT Hub en la que se creará la configuración. El centro debe estar en la suscripción actual. Cambie a la suscripción deseada con el comando `az account set -s [subscription name]`

* --**target-condition**: escriba una condición de destino para determinar qué dispositivos se dirigirán a esta configuración. La condición se basa en las etiquetas del dispositivo gemelo o en las propiedades deseadas del dispositivo gemelo y debe coincidir con el formato de expresión. Por ejemplo, `tags.environment='test'` o `properties.desired.devicemodel='4000x'`. 

* --**priority**: debe ser un entero positivo. En el caso de que dos o más configuraciones se destinen al mismo dispositivo, se aplicará la configuración que tenga el mayor valor numérico, según la prioridad.

* --**metrics**: ruta de acceso de archivo a las consultas de métrica. Las métricas proporcionan el número de resúmenes de los diferentes Estados que un dispositivo puede notificar después de aplicar el contenido de la configuración. Por ejemplo, puede crear una métrica para los cambios de configuración pendientes, una métrica para los errores y una métrica para los cambios de configuración correctos. 

## <a name="monitor-a-configuration"></a>Supervisión de una configuración

Use los comandos siguientes para ver el contenido de una configuración:

```cli
az iot hub configuration show --config-id [configuration id] \
  --hub-name [hub name]
```

* --**config-id**: el nombre de la configuración que está en IoT Hub.

* --**hub-name**: el nombre de la instancia de IoT Hub en la que está la configuración. El centro debe estar en la suscripción actual. Cambie a la suscripción deseada con el comando `az account set -s [subscription name]`

Revise la configuración en la ventana de comandos. La propiedad **metrics** enumera un recuento para cada métrica que evalúa cada centro:

* **targetedCount**: es una métrica del sistema que especifica la cantidad de dispositivos gemelos que hay en IoT Hub que coinciden con las condiciones del destino.

* **appliedCount**: es una métrica del sistema que especifica la cantidad de dispositivos cuyo contenido de destino se ha aplicado.

* **La métrica personalizada** -todas las métricas que haya definido son las métricas de usuario.

Puede mostrar una lista de identificadores u objetos de dispositivos para cada una de las métricas mediante el siguiente comando:

```cli
az iot hub configuration show-metric --config-id [configuration id] \
   --metric-id [metric id] --hub-name [hub name] --metric-type [type] 
```

* --**config-id**: el nombre de la implementación que está en IoT Hub.

* --**metric-id**: el nombre de la métrica de la cual quiere ver la lista de identificadores de dispositivos, por ejemplo `appliedCount`.

* --**hub-name**: nombre de la instancia de IoT Hub en la que está la implementación. El centro debe estar en la suscripción actual. Cambie a la suscripción que quiera usar con el comando `az account set -s [subscription name]`.

* --**metric-type**: el tipo de métrica puede ser `system` o `user`.  Las métricas del sistema son `targetedCount` y `appliedCount`. El resto de métricas son métricas de usuario.

## <a name="modify-a-configuration"></a>Modificación de una configuración

Cuando se modifica una configuración, los cambios se replican inmediatamente a todos los dispositivos seleccionados. 

Si actualiza la condición de destino, se producen las siguientes actualizaciones:

* Si un dispositivo gemelo no cumplía la antigua condición de destino, pero cumple la nueva condición de destino y esta configuración es la prioridad más alta para ese dispositivo gemelo, esta configuración se aplica al dispositivo gemelo. 

* Si un dispositivo gemelo ya no cumple la condición de destino, los ajustes de la configuración se eliminarán y el dispositivo gemelo se modificará con la configuración de la siguiente prioridad más alta. 

* Si un dispositivo gemelo que ejecuta actualmente esta configuración ya no cumple la condición de destino y tampoco la condición de destino de cualquier otra configuración, los ajustes de la configuración se eliminarán y no se realizará ningún otro cambio en el dispositivo gemelo. 

Use los comandos siguientes para actualizar una configuración:

```cli
az iot hub configuration update --config-id [configuration id] \
   --hub-name [hub name] --set [property1.property2='value']
```

* --**config-id**: el nombre de la configuración que está en IoT Hub.

* --**hub-name**: el nombre de la instancia de IoT Hub en la que está la configuración. El centro debe estar en la suscripción actual. Cambie a la suscripción que quiera usar con el comando `az account set -s [subscription name]`.

* --**set**: actualiza una propiedad de la configuración. Puede actualizar las propiedades siguientes:

    * targetCondition - por ejemplo `targetCondition=tags.location.state='Oregon'`

    * labels 

    * prioridad

## <a name="delete-a-configuration"></a>Eliminación de una configuración

Al eliminar una configuración, los dispositivos gemelos adoptan la configuración con la siguiente prioridad más alta. Si los dispositivos gemelos no cumplen la condición de destino de cualquier otra configuración, no se aplica ningún otro ajuste. 

Use los comandos siguientes para eliminar una configuración:

```cli
az iot hub configuration delete --config-id [configuration id] \
   --hub-name [hub name] 
```
* --**config-id**: el nombre de la configuración que está en IoT Hub.

* --**hub-name**: el nombre de la instancia de IoT Hub en la que está la configuración. El centro debe estar en la suscripción actual. Cambie a la suscripción que quiera usar con el comando `az account set -s [subscription name]`.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió a configurar y supervisar dispositivos IoT a escala. Siga estos vínculos para más información sobre la administración de Azure IoT Hub:

* [Administración de las identidades de dispositivo de IoT Hub de forma masiva](iot-hub-bulk-identity-mgmt.md)
* [Métricas de IoT Hub](iot-hub-metrics.md)
* [Supervisión de operaciones](iot-hub-operations-monitoring.md)

Para explorar aún más las funcionalidades de IoT Hub, consulte:

* [Guía del desarrollador de IoT Hub](iot-hub-devguide.md)
* [Implementar AI en dispositivos perimetrales con Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)

Para explorar el uso del servicio IoT Hub Device Provisioning para habilitar el aprovisionamiento Just-In-Time sin intervención del usuario, vea: 

* [Azure IoT Hub Device Provisioning Service](/azure/iot-dps)
