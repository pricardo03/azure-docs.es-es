---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 11/04/2019
ms.author: larryfr
ms.openlocfilehash: c288ea4a2c62999f27b3f07c39df40dae7ab1426
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2019
ms.locfileid: "74927004"
---
Las entradas del documento `deploymentconfig.json` se asignan a los parámetros de [LocalWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservicedeploymentconfiguration?view=azure-ml-py). En la tabla siguiente se describe la asignación entre las entidades del documento JSON y los parámetros del método:

| Entidad JSON | Parámetro del método | DESCRIPCIÓN |
| ----- | ----- | ----- |
| `computeType` | N/D | El destino de proceso. Para los destinos locales, el valor tiene que ser `local`. |
| `port` | `port` | Puerto local en el que se va a exponer el punto de conexión HTTP del servicio. |

Este elemento JSON es un ejemplo de la configuración de implementación que se puede usar con la CLI:

```json
{
    "computeType": "local",
    "port": 32267
}
```
