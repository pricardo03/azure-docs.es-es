---
title: 'CLI de Azure Service Fabric: sfctl mesh gateway'
description: Más información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para obtener y eliminar los recursos de la puerta de enlace de Service Fabric Mesh.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 3e398ff8a1a0a28fd9d0650a7c9d18ba04817d33
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76906021"
---
# <a name="sfctl-mesh-gateway"></a>sfctl mesh gateway
Obtiene y elimina recursos de mesh gateway.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| delete | Elimina el recurso de puerta de enlace. |
| list | Enumera todos los recursos de puerta de enlace. |
| show | Obtiene el recurso de puerta de enlace con el nombre especificado. |

## <a name="sfctl-mesh-gateway-delete"></a>sfctl mesh gateway delete
Elimina el recurso de puerta de enlace.

Elimina el recurso de puerta de enlace identificado por el nombre.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --name -n [Obligatorio] | El nombre del recurso de puerta de enlace. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-mesh-gateway-list"></a>sfctl mesh gateway list
Enumera todos los recursos de puerta de enlace.

Obtiene la información sobre todos los recursos de puerta de enlace de un grupo de recursos determinado. La información incluye la descripción y otras propiedades de la puerta de enlace.

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-mesh-gateway-show"></a>sfctl mesh gateway show
Obtiene el recurso de puerta de enlace con el nombre especificado.

Obtiene la información sobre el recurso de puerta de enlace con el nombre especificado. La información incluye la descripción y otras propiedades de la puerta de enlace.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --name -n [Obligatorio] | El nombre del recurso de puerta de enlace. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |


## <a name="next-steps"></a>Pasos siguientes
- [Configuración](service-fabric-cli.md) de la CLI de Service Fabric.
- Obtenga información sobre cómo utilizar la CLI de Service Fabric con los [scripts de ejemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).