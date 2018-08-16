---
title: 'CLI de Azure Service Fabric: sfctl container | Microsoft Docs'
description: Describe los comandos de sfctl container de la CLI de Service Fabric.
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
ms.date: 07/31/2018
ms.author: bikang
ms.openlocfilehash: 27108d27ee27346e4cba44e6778faff56df70a36
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495135"
---
# <a name="sfctl-container"></a>sfctl container
Ejecute los comandos relacionados con el contenedor en un nodo de clúster.

## <a name="commands"></a>Comandos:

|Get-Help|DESCRIPCIÓN|
| --- | --- |
| invoke-api | Invoque API de REST de contenedor. |
| logs | Recuperación de los registros de contenedores. |

## <a name="sfctl-container-invoke-api"></a>sfctl container invoke-api
Invoque API de REST de contenedor.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --application-id           [obligatorio] | Identidad de aplicación. |
| --code-package-instance-id [obligatorio] | Identificador de instancia de paquete de código, que se puede recuperar por "service code-package-list". |
| --code-package-name        [obligatorio] | Nombre del paquete de código. |
| --container-api-uri-path   [obligatorio] | Ruta acceso de URI de la API REST del contenedor, use "{id}" en lugar de identificador/nombre de contenedor. |
| --node-name [Obligatorio] | El nombre del nodo. |
| --service-manifest-name [Obligatorio] | Nombre de manifiesto de servicio. |
| --container-api-body | Cuerpo de la solicitud HTTP para la API REST del contenedor. |
| --container-api-content-type | Tipo de contenido para la API REST del contenedor, el valor predeterminado es "application/json". |
| --container-api-http-verb | Verbo HTTP para la API REST del contenedor, el valor predeterminado es GET. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-container-logs"></a>sfctl container logs
Recuperación de los registros de contenedores.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --application-id           [obligatorio] | Identidad de aplicación. |
| --code-package-instance-id [obligatorio] | Identificador de instancia de paquete de código, que se puede recuperar por "service code-package-list". |
| --code-package-name        [obligatorio] | Nombre del paquete de código. |
| --node-name [Obligatorio] | El nombre del nodo. |
| --service-manifest-name [Obligatorio] | Nombre de manifiesto de servicio. |
| --tail | Solo devuelve este número de líneas de registro desde el final de los registros. Especifique como valor un entero o todo para generar todas las líneas de registro en la salida. El valor predeterminado es "all". |
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