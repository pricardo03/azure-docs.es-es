---
title: Manualmente, detener o iniciar los contenedores en Azure Container Instances
description: Obtenga información sobre cómo detener o iniciar un grupo de contenedores en Azure Container Instances manualmente.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/15/2019
ms.author: danlep
ms.openlocfilehash: 8e62d106a42dfbec897e5e14cf68fd3d7fd823c4
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65070813"
---
# <a name="manually-stop-or-start-containers-in-azure-container-instances"></a>Manualmente, detener o iniciar los contenedores en Azure Container Instances

El [directiva de reinicio](container-instances-restart-policy.md) configuración de un grupo de contenedores determina cómo las instancias de contenedor iniciar o detener de forma predeterminada. Puede invalidar el valor predeterminado cuando se establece manualmente, detener o iniciar un grupo de contenedores.

## <a name="stop"></a>Stop

Detenga manualmente un grupo de contenedores de ejecución: por ejemplo, mediante el uso de la [stop de contenedor az] [ az-container-stop] comando o el portal de Azure. Para determinadas cargas de trabajo de contenedor, es posible que desea detener un grupo de contenedores de larga ejecución tras un período definido para ahorrar en costos. 

*Cuando un grupo de contenedores entra en estado detenido, finaliza y se recicla todos los contenedores del grupo. No conserva el estado de contenedor.*

Cuando se reciclan, los contenedores de la [recursos](container-instances-container-groups.md#resource-allocation) se cancela la asignación y la facturación se detiene el grupo de contenedores.

La acción de detención no tiene ningún efecto si el grupo de contenedores ya finalizado (lo que se encuentra en estado Succeeded o Failed). Por ejemplo, un grupo de contenedores con las tareas de una sola ejecución de contenedores que se ejecutó correctamente finaliza en el estado correcto. Intenta detener el grupo en que el estado no se cambia el estado. 

## <a name="start"></a>Iniciar

Cuando se detiene un grupo de contenedores: ya sea porque los contenedores finaliza por sí solas o se detuvo manualmente el grupo: puede iniciar los contenedores. Por ejemplo, use el [inicio del contenedor az] [ az-container-start] comando o el portal de Azure para iniciar manualmente los contenedores del grupo. Si se actualiza la imagen de contenedor de cualquier contenedor, se extrae una nueva imagen. 

Cuando se inicia un grupo de contenedores comienza una nueva implementación con la misma configuración de contenedor. Esta acción puede ayudarle a volver a utilizar rápidamente una configuración de grupo de contenedores conocida que funciona de la manera prevista. No tiene que crear un nuevo grupo de contenedores para ejecutar la misma carga de trabajo.

Todos los contenedores en un grupo de contenedores se inician mediante esta acción. No se puede iniciar un contenedor específico en el grupo.

Después de iniciar o reiniciar un grupo de contenedores manualmente, este se ejecuta según la configuración de la directiva de reinicio.
  
## <a name="restart"></a>Reinicio

Puede reiniciar un grupo de contenedores mientras se está ejecutando - por ejemplo, mediante el uso de la [reinicio de contenedor az] [ az-container-restart] comando. Esta acción permite reiniciar todos los contenedores del grupo. Si se actualiza la imagen de contenedor de cualquier contenedor, se extrae una nueva imagen. 

El reinicio de un grupo de contenedores es útil cuando desea solucionar un problema de implementación. Por ejemplo, si una limitación de recursos temporal impide que los contenedores se ejecuten correctamente, reiniciar el grupo puede solucionar el problema.

Esta acción reinicia todos los contenedores en un grupo de contenedores. No se puede reiniciar un contenedor específico en el grupo.

Después de reiniciar manualmente un grupo de contenedores, se ejecuta el grupo de contenedor según la configuración directiva de reinicio.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre [configuración de directiva de reinicio](container-instances-restart-policy.md) en Azure Container Instances.

Además de manualmente detener e iniciar un grupo de contenedores con la configuración existente, puede [actualizar la configuración de](container-instances-update.md) de un grupo de contenedores de ejecución.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-container-restart]: /cli/azure/container?view=azure-cli-latest#az-container-restart
[az-container-start]: /cli/azure/container?view=azure-cli-latest#az-container-start
[az-container-stop]: /cli/azure/container?view=azure-cli-latest#az-container-stop
