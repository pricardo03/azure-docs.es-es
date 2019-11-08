---
author: larryfr
ms.service: machine-learning
ms.topic: include
ms.date: 10/06/2019
ms.author: larryfr
ms.openlocfilehash: b3c1ad48da0ef28c73e3af5d1e30414534f4e94d
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73800117"
---
Las entradas del documento `deploymentconfig.json` se asignan a los parámetros de [AciWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciservicedeploymentconfiguration?view=azure-ml-py). En la tabla siguiente se describe la asignación entre las entidades del documento JSON y los parámetros del método:

| Entidad JSON | Parámetro del método | DESCRIPCIÓN |
| ----- | ----- | ----- |
| `computeType` | N/D | El destino de proceso. Para ACI, el valor debe ser `ACI`. |
| `containerResourceRequirements` | N/D | Contenedor de las entidades de CPU y memoria. |
| &emsp;&emsp;`cpu` | `cpu_cores` | Número de núcleos de CPU que se van a asignar. El valor predeterminado es `0.1`. |
| &emsp;&emsp;`memoryInGB` | `memory_gb` | Cantidad de memoria (en GB) que se va a asignar a este servicio web. El valor predeterminado es `0.5`. |
| `location` | `location` | Región de Azure en la que se implementará este servicio web. Si no se especifica, se usará la ubicación del área de trabajo. Puede encontrar más información sobre las regiones disponibles aquí: [Regiones de ACI](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=container-instances) |
| `authEnabled` | `auth_enabled` | Indica si se debe habilitar la autenticación para este servicio web. El valor predeterminado es False. |
| `sslEnabled` | `ssl_enabled` | Indica si se debe habilitar SSL para este servicio web. El valor predeterminado es False. |
| `appInsightsEnabled` | `enable_app_insights` | Indica si se debe habilitar AppInsights para este servicio web. El valor predeterminado es False. |
| `sslCertificate` | `ssl_cert_pem_file` | Archivo de certificado necesario si SSL está habilitado |
| `sslKey` | `ssl_key_pem_file` | Archivo de clave necesario si SSL está habilitado |
| `cname` | `ssl_cname` | CNAME si SSL está habilitado |
| `dnsNameLabel` | `dns_name_label` | Etiqueta del nombre DNS para el punto de conexión de puntuación. Si no se especifica, se generará una etiqueta de nombre DNS única para el punto de conexión de puntuación. |

El siguiente elemento JSON es un ejemplo de la configuración de implementación que se puede usar con la CLI:

```json
{
    "computeType": "aci",
    "containerResourceRequirements":
    {
        "cpu": 0.5,
        "memoryInGB": 1.0
    },
    "authEnabled": true,
    "sslEnabled": false,
    "appInsightsEnabled": false
}
```