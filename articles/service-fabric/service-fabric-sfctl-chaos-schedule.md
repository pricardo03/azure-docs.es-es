---
title: 'CLI de Azure Service Fabric: sfctl chaos schedule | Microsoft Docs'
description: Describe los comandos de sfctl chaos schedule de la CLI Service Fabric.
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
ms.openlocfilehash: 443a8186ac6e57360105e59e30f84db997cd2251
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2019
ms.locfileid: "72897553"
---
# <a name="sfctl-chaos-schedule"></a>sfctl chaos schedule
Obtenga y establezca la programación de chaos.

## <a name="commands"></a>Comandos:

|Get-Help|DESCRIPCIÓN|
| --- | --- |
| get | Obtenga el valor de Chaos Schedule que define cómo y cuándo se ejecuta Chaos. |
| set | Establezca la programación que usa Chaos. |

## <a name="sfctl-chaos-schedule-get"></a>sfctl chaos schedule get
Obtenga el valor de Chaos Schedule que define cómo y cuándo se ejecuta Chaos.

Obtiene la versión de Chaos Schedule en uso y la programación Chaos Schedule que define cuándo y cómo se ejecuta Chaos.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-chaos-schedule-set"></a>sfctl chaos schedule set
Establezca la programación que usa Chaos.

Chaos programará automáticamente ejecuciones según la programación Chaos Schedule. La programación de Chaos se actualizará si la versión proporcionada coincide con la versión del servidor. Al actualizar la programación de Chaos, la versión del servidor se incrementa en 1. La versión del servidor se ajustará de nuevo a 0 después de alcanzar un número grande. Si está ejecutando Chaos cuando se realiza esta llamada, se producirá un error en la llamada.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --chaos-parameters-dictionary | Lista codificada en JSON que representa una asignación de nombres de cadena para ChaosParameters que va a usar Jobs. |
| --expiry-date-utc | La fecha y hora en que se debe dejar de usar Schedule para programar Chaos.  Valor predeterminado\: 9999-12-31T23\:59\:59.999Z. |
| --jobs | Lista codificada en JSON de ChaosScheduleJobs que representa cuándo ejecutar Chaos y con qué parámetros se ejecuta Chaos. |
| --start-date-utc | La fecha y hora en que se debe empezar a usar Schedule para programar Chaos.  Valor predeterminado\: 1601-01-01T00\:00\:00.000Z. |
| --timeout -t | Valor predeterminado\: 60. |
| --version | El número de versión de Schedule. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --debug | Aumenta el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestra este mensaje de ayuda y sale. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumenta el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

### <a name="examples"></a>Ejemplos

El comando siguiente establece una programación (suponiendo que la programación actual tiene versión 0) que se inicia el 1 de enero de 2016 y expira el 1 de enero de 2038, ejecutando Chaos 24 horas al día, 7 días a la semana. Durante ese tiempo, se programará Chaos en el clúster.

    sfctl chaos schedule set --version 0 --start-date-utc "2016-01-01T00:00:00.000Z" --expiry-date-utc "2038-01-01T00:00:00.000Z"
    --chaos-parameters-dictionary
    [
    {
        "Key":"adhoc",
        "Value":{
            "MaxConcurrentFaults":3,
            "EnableMoveReplicaFaults":true,
            "ChaosTargetFilter":{
                "NodeTypeInclusionList":[
                "N0010Ref",
                "N0020Ref",
                "N0030Ref",
                "N0040Ref",
                "N0050Ref"
                ]
            },
            "MaxClusterStabilizationTimeoutInSeconds":60,
            "WaitTimeBetweenIterationsInSeconds":15,
            "WaitTimeBetweenFaultsInSeconds":30,
            "TimeToRunInSeconds":"600",
            "Context":{
                "Map":{
                "test":"value"
                }
            },
            "ClusterHealthPolicy":{
                "MaxPercentUnhealthyNodes":0,
                "ConsiderWarningAsError":true,
                "MaxPercentUnhealthyApplications":0
            }
        }
    }
    ]
    --jobs
    [
    {
        "ChaosParameters":"adhoc",
        "Days":{
            "Sunday":true,
            "Monday":true,
            "Tuesday":true,
            "Wednesday":true,
            "Thursday":true,
            "Friday":true,
            "Saturday":true
        },
        "Times":[
            {
                "StartTime":{
                "Hour":0,
                "Minute":0
                },
                "EndTime":{
                "Hour":23,
                "Minute":59
                }
            }
        ]
    }
    ]


## <a name="next-steps"></a>Pasos siguientes
- [Configuración](service-fabric-cli.md) de la CLI de Service Fabric.
- Obtenga información sobre cómo utilizar la CLI de Service Fabric con los [scripts de ejemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).
