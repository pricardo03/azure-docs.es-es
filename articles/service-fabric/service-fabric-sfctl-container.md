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
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: 455b2a70568566bff5b1ea4c185568a1758f7db3
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53274911"
---
# <a name="sfctl-container"></a>sfctl container
Ejecute los comandos relacionados con el contenedor en un nodo de clúster.

## <a name="commands"></a>Comandos:

|Get-Help|DESCRIPCIÓN|
| --- | --- |
| invoke-api | Invoca la API de contenedor para el contenedor implementado en un nodo de Service Fabric para el paquete de código especificado. |
| logs | Obtiene los registros de contenedor para el contenedor implementado en un nodo de Service Fabric para el paquete de código especificado. |

## <a name="sfctl-container-invoke-api"></a>sfctl container invoke-api
Invoca la API de contenedor para el contenedor implementado en un nodo de Service Fabric para el paquete de código especificado.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --application-id           [obligatorio] | La identidad de la aplicación. <br><br> Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --code-package-instance-id [obligatorio] | Identificador que de forma exclusiva identifica a una instancia de paquete de código implementada en un nodo de Service Fabric. <br><br> Se puede recuperar con "service code-package-list". |
| --code-package-name        [obligatorio] | El nombre del paquete de código especificado en un manifiesto de servicio registrado como parte de un tipo de aplicación en un clúster de Service Fabric. |
| --container-api-uri-path   [obligatorio] | Ruta acceso de URI de la API REST del contenedor, use "{id}" en lugar de identificador/nombre de contenedor. |
| --node-name [Obligatorio] | El nombre del nodo. |
| --service-manifest-name [Obligatorio] | El nombre de un manifiesto de servicio registrado como parte de un tipo de aplicación en un clúster de Service Fabric. |
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
Obtiene los registros de contenedor para el contenedor implementado en un nodo de Service Fabric para el paquete de código especificado.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --application-id           [obligatorio] | La identidad de la aplicación. <br><br> Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --code-package-instance-id [obligatorio] | Identificador de instancia de paquete de código, que se puede recuperar por "service code-package-list". |
| --code-package-name        [obligatorio] | El nombre del paquete de código especificado en un manifiesto de servicio registrado como parte de un tipo de aplicación en un clúster de Service Fabric. |
| --node-name [Obligatorio] | El nombre del nodo. |
| --service-manifest-name [Obligatorio] | El nombre de un manifiesto de servicio registrado como parte de un tipo de aplicación en un clúster de Service Fabric. |
| --tail | Número de líneas para mostrar desde el final de los registros. El valor predeterminado es 100. "all" para mostrar los registros completos. |
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