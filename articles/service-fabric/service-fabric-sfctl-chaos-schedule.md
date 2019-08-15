---
title: 'CLI de Azure Service Fabric: sfctl chaos schedule | Microsoft Docs'
description: Describe los comandos de sfctl chaos schedule de la CLI Service Fabric.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: 1664978110b7c700906cbf4e6c80806ac70f1f05
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69036528"
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
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |

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

Chaos programará automáticamente ejecuciones según la programación Chaos Schedule. La versión en la programación de entrada proporcionada debe coincidir con la versión de Chaos Schedule del servidor. Si la versión proporcionada no coincide con la versión del servidor, no se actualiza Chaos Schedule. Si la versión que se proporciona coincide con la versión del servidor, se actualiza Chaos Schedule y la versión de Chaos Schedule en el servidor se incrementa en uno y se ajusta hacia 0 después de 2,147,483,647. Si está ejecutando Chaos cuando se realiza esta llamada, se producirá un error en la llamada.

### <a name="arguments"></a>Argumentos

|Argumento|DESCRIPCIÓN|
| --- | --- |
| --chaos-parameters-dictionary | Lista codificada en JSON que representa una asignación de nombres de cadena para ChaosParameters que va a usar Jobs. |
| --expiry-date-utc | La fecha y hora en que se debe dejar de usar Schedule para programar Chaos.  Valor predeterminado\: 9999-12-31T23\:59\:59.999Z. |
| --jobs | Lista codificada en JSON de ChaosScheduleJobs que representa cuándo ejecutar Chaos y con qué parámetros se ejecuta Chaos. |
| --start-date-utc | La fecha y hora en que se debe empezar a usar Schedule para programar Chaos.  Valor predeterminado\: 1601-01-01T00\:00\:00.000Z. |
| --timeout -t | Tiempo de espera del servidor en segundos.  Valor predeterminado\: 60. |
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
