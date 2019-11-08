---
title: Actividad de filtro en Azure Data Factory
description: La actividad de filtro filtra las entradas.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/04/2018
ms.openlocfilehash: 5794a2c754e2082fdf45ad689bdb89ea494541c2
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73679854"
---
# <a name="filter-activity-in-azure-data-factory"></a>Actividad de filtro en Azure Data Factory
Puede usar una actividad de filtro en una canalización para aplicar una expresión de filtro a una matriz de entrada. 

## <a name="syntax"></a>Sintaxis

```json
{
    "name": "MyFilterActivity",
    "type": "filter",
    "typeProperties": {
        "condition": "<condition>",
        "items": "<input array>"
    }
}
```

## <a name="type-properties"></a>Propiedades de tipo

Propiedad | DESCRIPCIÓN | Valores permitidos | Obligatorio
-------- | ----------- | -------------- | --------
Nombre | Nombre de la actividad `Filter`. | Cadena | Sí
Tipo | Se debe establecer en **filter**. | Cadena | Sí
condition | Condición que se usará para filtrar la entrada. | Expression | Sí
items | Matriz de entrada en la que se debe aplicar el filtro. | Expression | Sí

## <a name="example"></a>Ejemplo

En este ejemplo, la canalización tiene dos actividades: **Filter** y **ForEach**. La actividad de filtro está configurada para filtrar la matriz de entrada correspondiente a los elementos con un valor mayor que 3. La actividad "Para cada uno" recorre en iteración los valores filtrados y establece la variable **test** en el valor actual.

```json
{
    "name": "PipelineName",
    "properties": {
        "activities": [{
                "name": "MyFilterActivity",
                "type": "filter",
                "typeProperties": {
                    "condition": "@greater(item(),3)",
                    "items": "@pipeline().parameters.inputs"
                }
            },
            {
            "name": "MyForEach",
            "type": "ForEach",
            "dependsOn": [
                {
                    "activity": "MyFilterActivity",
                    "dependencyConditions": [
                        "Succeeded"
                    ]
                }
            ],
            "userProperties": [],
            "typeProperties": {
                "items": {
                    "value": "@activity('MyFilterActivity').output.value",
                    "type": "Expression"
                },
                "isSequential": "false",
                "batchCount": 1,
                "activities": [
                    {
                        "name": "Set Variable1",
                        "type": "SetVariable",
                        "dependsOn": [],
                        "userProperties": [],
                        "typeProperties": {
                            "variableName": "test",
                            "value": {
                                "value": "@string(item())",
                                "type": "Expression"
                            }
                        }
                    }
                ]
            }
        }],
        "parameters": {
            "inputs": {
                "type": "Array",
                "defaultValue": [1, 2, 3, 4, 5, 6]
            }
        },
        "variables": {
            "test": {
                "type": "String"
            }
        },
        "annotations": []
    }
}
```

## <a name="next-steps"></a>Pasos siguientes
Consulte otras actividades de flujo de control compatibles con Data Factory: 

- [Actividad If Condition](control-flow-if-condition-activity.md)
- [Actividad de ejecución de canalización](control-flow-execute-pipeline-activity.md)
- [Para cada actividad](control-flow-for-each-activity.md)
- [Actividad de obtención de metadatos](control-flow-get-metadata-activity.md)
- [Actividad Lookup](control-flow-lookup-activity.md)
- [Actividad web](control-flow-web-activity.md)
- [Actividad Until](control-flow-until-activity.md)
