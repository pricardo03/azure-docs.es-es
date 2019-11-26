---
title: 'CLI de Azure Service Fabric: sfctl mesh secretvalue | Microsoft Docs'
description: Se describen los comandos de sfctl mesh secretvalue de la CLI de Service Fabric.
services: service-fabric
documentationcenter: na
author: jeffj6123
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: 96ce4efe00d89c136a0c11d445170b2f67be6fcd
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2019
ms.locfileid: "72901191"
---
# <a name="sfctl-mesh-secretvalue"></a>sfctl mesh secretvalue
Obtiene y elimina recursos de mesh secretvalue.

## <a name="commands"></a>Comandos:

|Get-Help|DESCRIPCIÓN|
| --- | --- |
| delete | Elimina el valor especificado del recurso de secreto con nombre. |
| list | Enumera los nombres de todos los valores del recurso de secreto especificado. |
| show | Enumera el valor especificado del recurso de secreto. |

## <a name="sfctl-mesh-secretvalue-delete"></a>sfctl mesh secretvalue delete
Elimina el valor especificado del recurso de secreto con nombre.

Elimina el recurso del valor de secreto identificado por el nombre. El nombre del recurso suele ser la versión asociada con ese valor. La eliminación no se producirá si el valor especificado está en uso.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --secret-name -n [obligatorio] | Nombre del recurso de secreto. |
| --version -v     [obligatorio] | Nombre de la versión del secreto. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-mesh-secretvalue-list"></a>sfctl mesh secretvalue list
Enumera los nombres de todos los valores del recurso de secreto especificado.

Obtiene información sobre todos los recursos de valor de secreto del recurso de secreto especificado. La información incluye los nombres de los recursos de valor de secreto, pero no los valores reales.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --secret-name -n [obligatorio] | Nombre del recurso de secreto. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-mesh-secretvalue-show"></a>sfctl mesh secretvalue show
Enumera el valor especificado del recurso de secreto.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --secret-name -n [obligatorio] | Nombre del recurso de secreto. |
| --version -v     [obligatorio] | Nombre de la versión del secreto. |
| --show-value | Muestra el valor real de la versión de secreto. |

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