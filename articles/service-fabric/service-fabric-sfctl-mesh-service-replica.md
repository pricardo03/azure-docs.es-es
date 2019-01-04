---
title: 'CLI de Azure Service Fabric: sfctl mesh service-replica | Microsoft Docs'
description: Se describen los comandos de sfctl mesh service-replica de la CLI de Service Fabric.
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
ms.openlocfilehash: bcf4b8d013783a9fbdb62bcdb8737680bfce7640
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53283331"
---
# <a name="sfctl-mesh-service-replica"></a>sfctl mesh service-replica
Obtiene los detalles de la réplica y muestra las réplicas de un servicio determinado en un recurso de aplicación.

## <a name="commands"></a>Comandos:

|Get-Help|DESCRIPCIÓN|
| --- | --- |
| list | Enumera todas las réplicas de un servicio. |
| show | Obtiene la réplica dada del servicio de una aplicación. |

## <a name="sfctl-mesh-service-replica-list"></a>sfctl mesh service-replica list
Enumera todas las réplicas de un servicio.

Obtiene la información sobre todas las réplicas de un servicio. La información incluye la descripción y otras propiedades de la réplica del servicio.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --app-name: nombre de la aplicación [obligatorio] | Nombre de la aplicación. |
| --service-name [obligatorio] | El nombre del servicio. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-mesh-service-replica-show"></a>sfctl mesh service-replica show
Obtiene la réplica dada del servicio de una aplicación.

Obtiene la información sobre la réplica de servicio con el nombre especificado. La información incluye la descripción y otras propiedades de la réplica del servicio.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --app-name: nombre de la aplicación [obligatorio] | Nombre de la aplicación. |
| --name -n [obligatorio] | El nombre de la réplica de servicio. |
| --service-name [obligatorio] | El nombre del servicio. |

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