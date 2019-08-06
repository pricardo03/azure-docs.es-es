---
author: larryfr
ms.service: machine-learning
ms.topic: include
ms.date: 07/26/2019
ms.author: larryfr
ms.openlocfilehash: a87773c8f6ddda11b62064758b46d666740a03c2
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68556943"
---
Las entradas del documento `deploymentconfig.json` se asignan a los parámetros de [LocalWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservicedeploymentconfiguration?view=azure-ml-py). En la tabla siguiente se describe la asignación entre las entidades del documento JSON y los parámetros del método:

| Entidad JSON | Parámetro de método | DESCRIPCIÓN |
| ----- | ----- | ----- |
| `computeType` | N/D | El destino de proceso. Para una implementación local, el valor tiene que ser `local`. |
| `port` | `port` | Puerto local en el que se va a exponer el punto de conexión HTTP del servicio. |

El siguiente JSON es un ejemplo de la configuración de implementación que se puede usar con la CLI:

```json
{
    "computeType": "local",
    "port": 32267
}
```
