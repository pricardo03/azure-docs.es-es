---
title: Referencia de Azure IoT EdgeAgent y EdgeHub | Microsoft Docs
description: Revisión de las propiedades específicas y sus valores de los módulos gemelos edgeAgent y edgeHub
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ecc48adfeef30a777ae4d96c9b996c8bcdfea12d
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50247817"
---
# <a name="properties-of-the-edge-agent-and-edge-hub-module-twins"></a>Propiedades de los módulos gemelos EdgeAgent y EdgeHub

EdgeAgent y EdgeHub son dos módulos que constituyen el entorno de tiempo de ejecución de IoT Edge. Para más información acerca de las tareas que realiza cada módulo, consulte [Información sobre el entorno de ejecución de Azure IoT Edge y su arquitectura](iot-edge-runtime.md). 

En este artículo se proporcionan las propiedades deseadas y notificadas de los módulos gemelos de tiempo de ejecución. Para más información sobre cómo implementar módulos en dispositivos IoT Edge, consulte el artículo sobre [implementación y supervisión](module-deployment-monitoring.md).

## <a name="edgeagent-desired-properties"></a>Propiedades deseadas de EdgeAgent

El módulo gemelo del agente de Edge se denomina `$edgeAgent` y coordina las comunicaciones entre el agente de Edge que se ejecuta en un dispositivo y en IoT Hub. Las propiedades deseadas se establecen al aplicar un manifiesto de implementación en un dispositivo específico como parte de la implementación de un solo dispositivo o a escala. 

| Propiedad | DESCRIPCIÓN | Obligatorio |
| -------- | ----------- | -------- |
| schemaVersion | Debe ser "1.0". | SÍ |
| runtime.type | Debe ser "docker". | SÍ |
| runtime.settings.minDockerVersion | Establece la propiedad en la versión de Docker mínima que requiere este manifiesto de implementación. | SÍ |
| runtime.settings.loggingOptions | Cadenas JSON que contienen las opciones de registro del contenedor del agente de Edge. [Opciones de registro de Docker](https://docs.docker.com/engine/admin/logging/overview/) | Sin  |
| runtime.settings.registryCredentials<br>.{registryId}.username | El nombre de usuario del registro de contenedor. Para Azure Container Registry, el nombre de usuario suele ser el nombre del registro.<br><br> Las credenciales del registro son necesarias para las imágenes de módulo que no son públicas. | Sin  |
| runtime.settings.registryCredentials<br>.{registryId}.password | La contraseña del registro de contenedor. | Sin  |
| runtime.settings.registryCredentials<br>.{registryId}.address | La dirección del registro de contenedor. Para Azure Container Registry, la dirección suele ser *{registryname}.azurecr.io*. | Sin  |  
| systemModules.edgeAgent.type | Debe ser "docker". | SÍ |
| systemModules.edgeAgent.settings.image | El URI de la imagen del agente de Edge. Actualmente, el agente de Edge no puede actualizarse automáticamente. | SÍ |
| systemModules.edgeAgent.settings<br>.createOptions | Cadenas JSON que contienen las opciones de creación del contenedor del agente de Edge. [Opciones de creación de Docker](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Sin  |
| systemModules.edgeAgent.configuration.id | El identificador de la implementación que implementó este módulo. | Esta propiedad la establece IoT Hub cuando se aplica este manifiesto mediante una implementación. No forma parte de un manifiesto de implementación. |
| systemModules.edgeHub.type | Debe ser "docker". | SÍ |
| systemModules.edgeHub.status | Debe ser "running". | SÍ |
| systemModules.edgeHub.restartPolicy | Debe ser "always". | SÍ |
| systemModules.edgeHub.settings.image | El URI de la imagen del centro de Edge. | SÍ |
| systemModules.edgeHub.settings<br>.createOptions | Cadenas JSON que contienen las opciones de creación del contenedor del centro de Edge. [Opciones de creación de Docker](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Sin  |
| systemModules.edgeHub.configuration.id | El identificador de la implementación que implementó este módulo. | Esta propiedad la establece IoT Hub cuando se aplica este manifiesto mediante una implementación. No forma parte de un manifiesto de implementación. |
| modules.{moduleId}.version | Una cadena definida por el usuario que representa la versión de este módulo. | SÍ |
| modules.{moduleId}.type | Debe ser "docker". | SÍ |
| modules.{moduleId}.status | {"running" \| "stopped"} | SÍ |
| modules.{moduleId}.restartPolicy | {"never" \| "on-failed" \| "on-unhealthy" \| "always"} | SÍ |
| modules.{moduleId}.settings.image | El URI de la imagen del módulo. | SÍ |
| modules.{moduleId}.settings.createOptions | Cadenas JSON que contienen las opciones de creación del contenedor del módulo. [Opciones de creación de Docker](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Sin  |
| modules.{moduleId}.configuration.id | El identificador de la implementación que implementó este módulo. | Esta propiedad la establece IoT Hub cuando se aplica este manifiesto mediante una implementación. No forma parte de un manifiesto de implementación. |

## <a name="edgeagent-reported-properties"></a>Propiedades notificadas de EdgeAgent

Las propiedades notificadas del agente de Edge incluyen tres fragmentos principales de información:

1. El estado de la aplicación de las propiedades deseadas procesadas por última vez.
2. El estado de los módulos que se están ejecutando actualmente en el dispositivo, tal y como ha notificado el agente de Edge.
3. Una copia de las propiedades deseadas que se están ejecutando actualmente en el dispositivo.

Este último fragmento de información es útil en caso de que las propiedades deseadas más recientes no las aplicara correctamente la instancia en tiempo de ejecución, y de que el dispositivo aún esté ejecutando un manifiesto de implementación anterior.

> [!NOTE]
> Las propiedades notificadas del agente de Edge resultan útiles porque pueden consultarse con el [lenguaje de consultas de IoT Hub](../iot-hub/iot-hub-devguide-query-language.md) para investigar a escala el estado de las implementaciones. Para más información sobre cómo usar las propiedades del agente de Edge para ver el estado, consulte [Descripción de las implementaciones de IoT Edge en un único dispositivo o a escala](module-deployment-monitoring.md).

La tabla siguiente no incluye la información que se copia de las propiedades deseadas.

| Propiedad | DESCRIPCIÓN |
| -------- | ----------- |
| lastDesiredVersion | Este entero hace referencia a la última versión de las propiedades deseadas procesadas mediante el agente de Edge. |
| lastDesiredStatus.code | Se trata del código de estado que hace referencia a las últimas propiedades que procesó el agente de Edge. Valores permitidos: `200` (correcto), `400` (configuración no válida), `412` (versión de esquema no válido), `417` (las propiedades deseadas están vacías) y `500` (error). |
| lastDesiredStatus.description | Descripción de texto del estado. |
| deviceHealth | `healthy` si el estado en tiempo de ejecución de todos los módulos es `running` o `stopped`; `unhealthy` otra condición. |
| configurationHealth.{deploymentId}.health | `healthy` si el estado en tiempo de ejecución de todos los módulos establecidos mediante la implementación {deploymentId} es `running` o `stopped`; `unhealthy` otra condición. |
| runtime.platform.OS | Notifica el sistema operativo que se ejecuta en el dispositivo. |
| runtime.platform.architecture | Notifica la arquitectura de la CPU del dispositivo. |
| systemModules.edgeAgent.runtimeStatus | El estado notificado del agente de Edge: {"running" \| "unhealthy"} |
| systemModules.edgeAgent.statusDescription | Descripción de texto del estado notificado del agente de Edge. |
| systemModules.edgeHub.runtimeStatus | Estado actual del centro de Microsoft Edge: { "running" \| "stopped" \| "failed" \| "backoff" \| "unhealthy" } |
| systemModules.edgeHub.statusDescription | Descripción de texto del estado actual del centro de Edge si es el estado es unhealthy. |
| systemModules.edgeHub.exitCode | Si se produce un evento de salida, será el código de salida que notifica el contenedor del centro de Edge. |
| systemModules.edgeHub.startTimeUtc | Hora a la que se inició por última vez el centro de Edge. |
| systemModules.edgeHub.lastExitTimeUtc | Hora a la que se salió por última vez del centro de Edge. |
| systemModules.edgeHub.lastRestartTimeUtc | Hora a la que se reinició por última vez el centro de Edge. |
| systemModules.edgeHub.restartCount | Número de veces que se ha reiniciado este módulo como parte de la directiva de reinicio. |
| modules.{moduleId}.runtimeStatus | Estado actual del módulo: { "running" \| "stopped" \| "failed" \| "backoff" \| "unhealthy" } |
| modules.{moduleId}.statusDescription | Descripción de texto del estado actual del módulo si el estado es unhealthy. |
| modules.{moduleId}.exitCode | Si se produce un evento de salida, será el código de salida que notifica el contenedor del módulo. |
| modules.{moduleId}.startTimeUtc | Hora a la que se inició por última vez el módulo. |
| modules.{moduleId}.lastExitTimeUtc | Hora a la que se salió por última vez del módulo. |
| modules.{moduleId}.lastRestartTimeUtc | Hora a la que se reinició por última vez el módulo. |
| modules.{moduleId}.restartCount | Número de veces que se ha reiniciado este módulo como parte de la directiva de reinicio. |

## <a name="edgehub-desired-properties"></a>Propiedades deseadas de EdgeHub

El módulo gemelo del centro de Edge se denomina `$edgeHub` y coordina las comunicaciones entre el centro de Edge que se ejecuta en un dispositivo y en IoT Hub. Las propiedades deseadas se establecen al aplicar un manifiesto de implementación en un dispositivo específico como parte de la implementación de un solo dispositivo o a escala. 

| Propiedad | DESCRIPCIÓN | Requerida en el manifiesto de implementación |
| -------- | ----------- | -------- |
| schemaVersion | Debe ser "1.0". | SÍ |
| routes.{routeName} | Una cadena que representa una ruta del centro de Edge. | El elemento `routes` puede estar presente, pero vacío. |
| storeAndForwardConfiguration.timeToLiveSecs | El tiempo en segundos que el centro de Microsoft Edge conserva los mensajes en el caso de que se desconecten puntos de conexión de enrutamiento, por ejemplo, de IoT Hub o del módulo local. | SÍ |

## <a name="edgehub-reported-properties"></a>Propiedades notificadas de EdgeHub

| Propiedad | DESCRIPCIÓN |
| -------- | ----------- |
| lastDesiredVersion | Este entero hace referencia a la última versión de las propiedades deseadas procesadas mediante el centro de Microsoft Edge. |
| lastDesiredStatus.code | Se trata del código de estado que hace referencia a las últimas propiedades que procesó el centro de Edge. Valores permitidos: `200` (correcto), `400` (configuración no válida) y `500` (error). |
| lastDesiredStatus.description | Descripción de texto del estado. |
| clients.{device or moduleId}.status | El estado de conectividad de este dispositivo o módulo. Valores posibles {"connected" \| "disconnected"}. Solo las identidades de módulo pueden estar en el estado disconnected. Los dispositivos de nivel inferior que se conectan al centro de Edge solo aparecen cuando se conectan. |
| clients.{device or moduleId}.lastConnectTime | Última vez que se conectó el dispositivo o módulo. |
| clients.{device or moduleId}.lastDisconnectTime | Última vez que se desconectó el dispositivo o módulo. |

## <a name="next-steps"></a>Pasos siguientes

Para aprender a usar estas propiedades para crear manifiestos de implementación, consulte [Descripción de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge ](module-composition.md).
