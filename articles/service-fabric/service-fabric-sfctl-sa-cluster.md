---
title: 'CLI de Azure Service Fabric: sfctl sa-cluster | Microsoft Docs'
description: Describe los comandos de clúster independientes de sfctl de la CLI de Service Fabric.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 05/23/2018
ms.author: bikang
ms.openlocfilehash: ffdbff7edc5af187071615c8b1e61790b3a38429
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763984"
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

Obtenga la configuración del clúster independiente de Service Fabric. La configuración de clúster contiene propiedades del clúster que incluyen distintos tipos de nodos en el clúster, las configuraciones de seguridad, los errores y las topologías de dominio de actualización, etc.

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
| --delta-unhealthy-nodes | El porcentaje máximo permitido de degradación de mantenimiento delta durante la actualización. Los valores permitidos son enteros entre 0 y 100. |
| --health-check-retry | El período de tiempo entre intentos para realizar comprobaciones de mantenimiento si la aplicación o el clúster no son correctos.  Valor predeterminado\: PT0H0M0S. |
| --health-check-stable | El período de tiempo que la aplicación o el clúster debe permanecer en buen estado.  Valor predeterminado\: PT0H0M0S. |
| --health-check-wait | El período de tiempo de espera después de completar un dominio de actualización antes de iniciar el proceso de comprobaciones de mantenimiento.  Valor predeterminado\: PT0H0M0S. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |
| --unhealthy-applications | El porcentaje máximo permitido de aplicaciones en mal estado durante la actualización. Los valores permitidos son enteros entre 0 y 100. |
| --unhealthy-nodes | El porcentaje máximo permitido de nodos en mal estado durante la actualización. Los valores permitidos son enteros entre 0 y 100. |
| --upgrade-domain-delta-unhealthy-nodes | El porcentaje máximo permitido de degradación de mantenimiento delta de dominio de actualización durante la actualización. Los valores permitidos son enteros entre 0 y 100. |
| --upgrade-domain-timeout | El tiempo de espera para el dominio de actualización.  Valor predeterminado\: PT0H0M0S. |
| --upgrade-timeout | El tiempo de espera de actualización.  Valor predeterminado\: PT0H0M0S. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

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