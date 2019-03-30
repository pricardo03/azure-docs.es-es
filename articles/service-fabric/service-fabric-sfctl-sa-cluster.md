---
title: 'CLI de Azure Service Fabric: sfctl sa-cluster | Microsoft Docs'
description: Describe los comandos de clúster independientes de sfctl de la CLI de Service Fabric.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: a652439729e538b3ce2545ab3b09284e6645ce9d
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58668524"
---
# <a name="sfctl-sa-cluster"></a>sfctl sa-cluster
Administre clústeres de Service Fabric independientes.

## <a name="commands"></a>Comandos:

|Get-Help|DESCRIPCIÓN|
| --- | --- |
| config | Obtenga la configuración del clúster independiente de Service Fabric. |
| config-upgrade | Comience a actualizar la configuración de un clúster independiente de Service Fabric. |
| upgrade-status | Obtenga el estado de actualización de configuración de clúster de un clúster de Service Fabric independiente. |

## <a name="sfctl-sa-cluster-config"></a>sfctl sa-cluster config
Obtenga la configuración del clúster independiente de Service Fabric.

La configuración de clúster contiene propiedades del clúster que incluyen distintos tipos de nodos en el clúster, las configuraciones de seguridad, los errores y las topologías de dominio de actualización, etc.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --configuration-api-version [obligatorio] | La versión de API de la configuración de json de clúster independiente. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-sa-cluster-config-upgrade"></a>sfctl sa-cluster config-upgrade
Comience a actualizar la configuración de un clúster independiente de Service Fabric.

Valide los parámetros de actualización de configuración suministrados e inicie la actualización de la configuración de un clúster si los parámetros son válidos.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --cluster-config            [obligatorio] | La configuración de clúster. |
| --application-health-policies | Diccionario codificado en JSON de pares de nombres de tipo de aplicación y porcentaje máximo con estado incorrecto antes de producirse el error. |
| --delta-unhealthy-nodes | El porcentaje máximo permitido de degradación de mantenimiento delta durante la actualización. Los valores permitidos son enteros entre 0 y 100. |
| --health-check-retry | El período de tiempo entre intentos para realizar comprobaciones de mantenimiento si la aplicación o el clúster no funcionan correctamente.  Valor predeterminado\: PT0H0M0S. |
| --health-check-stable | La cantidad de tiempo que la aplicación o el clúster deben tener un estado correcto antes de que la actualización continúe con el siguiente dominio de actualización.  Valor predeterminado\: PT0H0M0S. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |
| --health-check-wait | El período de tiempo de espera después de completar un dominio de actualización antes de iniciar el proceso de comprobaciones de mantenimiento.  Valor predeterminado\: PT0H0M0S. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |
| --unhealthy-applications | El porcentaje máximo permitido de aplicaciones en mal estado durante la actualización. Los valores permitidos son enteros entre 0 y 100. |
| --unhealthy-nodes | El porcentaje máximo permitido de nodos en mal estado durante la actualización. Los valores permitidos son enteros entre 0 y 100. |
| --upgrade-domain-delta-unhealthy-nodes | El porcentaje máximo permitido de degradación de mantenimiento delta de dominio de actualización durante la actualización. Los valores permitidos son enteros entre 0 y 100. |
| --upgrade-domain-timeout | El período de tiempo del que dispone cada dominio de actualización para completarse antes de la ejecución de FailureAction.  Valor predeterminado\: PT0H0M0S. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |
| --upgrade-timeout | El período de tiempo en el que se debe completar la actualización general antes de que se ejecute FailureAction.  Valor predeterminado\: PT0H0M0S. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

### <a name="examples"></a>Ejemplos

Iniciar una actualización de la configuración de clúster

```
sfctl sa-cluster config-upgrade --cluster-config <YOUR CLUSTER CONFIG> --application-health-
policies "{"fabric:/System":{"ConsiderWarningAsError":true}}"
```

## <a name="sfctl-sa-cluster-upgrade-status"></a>sfctl sa-cluster upgrade-status
Obtenga el estado de actualización de configuración de clúster de un clúster de Service Fabric independiente.

Obtenga los detalles del estado de actualización de configuración de clúster de un clúster de Service Fabric independiente.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |


## <a name="next-steps"></a>Pasos siguientes
- [Configuración](service-fabric-cli.md) de la CLI de Service Fabric.
- Obtenga información sobre cómo utilizar la CLI de Service Fabric con los [scripts de ejemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).