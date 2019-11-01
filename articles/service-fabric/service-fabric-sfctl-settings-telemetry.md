---
title: 'CLI de Azure Service Fabric CLI: telemetría de configuración de sfctl | Microsoft Docs'
description: Se describen los comandos de telemetría de configuración de sfctl de la CLI de Service Fabric.
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
ms.openlocfilehash: d926c71ae8fd9e196b86c14c7fb96cc65b587b65
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2019
ms.locfileid: "72900975"
---
# <a name="sfctl-settings-telemetry"></a>sfctl settings telemetry
Configura los valores de telemetría locales para esta instancia de sfctl.

La telemetría de sfctl recopila el nombre del comando sin que se proporcionen los parámetros ni sus valores, la versión de sfctl, el tipo de sistema operativo, la versión de Python, el éxito o error del comando o el mensaje de error devuelto.

## <a name="commands"></a>Comandos:

|Get-Help|DESCRIPCIÓN|
| --- | --- |
| set-telemetry | Activar o desactivar la telemetría. |

## <a name="sfctl-settings-telemetry-set-telemetry"></a>sfctl settings telemetry set-telemetry
Activar o desactivar la telemetría.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --off | Desactivar la telemetría. |
| --on | Activar la telemetría. Este es el valor predeterminado. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

### <a name="examples"></a>Ejemplos

Desactivar la telemetría.

```
sfctl settings telemetry set_telemetry --off
```

Activar la telemetría.

```
sfctl settings telemetry set_telemetry --on
```


## <a name="next-steps"></a>Pasos siguientes
- [Configuración](service-fabric-cli.md) de la CLI de Service Fabric.
- Obtenga información sobre cómo utilizar la CLI de Service Fabric con los [scripts de ejemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).