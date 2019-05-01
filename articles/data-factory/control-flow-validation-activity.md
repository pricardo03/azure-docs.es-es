---
title: Actividad de validación en Azure Data Factory | Microsoft Docs
description: La actividad de validación no continuar la ejecución de la canalización hasta que valida el conjunto de datos conectado con determinados criterios que el usuario especifica.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: shlo
ms.openlocfilehash: 46447bdbea93d1f99c5682cf878c2035e6f49b78
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60764329"
---
# <a name="validation-activity-in-azure-data-factory"></a>Actividad de validación en Azure Data Factory
Puede usar una validación para asegurarse de que la canalización solo continúa la ejecución una vez que se ha validado el archivo adjunto en una canalización de referencia de conjunto de datos existe, que cumpla los criterios especificados o se ha alcanzado el tiempo de espera.


## <a name="syntax"></a>Sintaxis

```json

{
    "name": "Validation_Activity",
    "type": "Validation",
    "typeProperties": {
        "dataset": {
            "referenceName": "Storage_File",
            "type": "DatasetReference"
        },
        "timeout": "7.00:00:00",
        "sleep": 10,
        "minimumSize": 20
    }
},
{
    "name": "Validation_Activity_Folder",
    "type": "Validation",
    "typeProperties": {
        "dataset": {
            "referenceName": "Storage_Folder",
            "type": "DatasetReference"
        },
        "timeout": "7.00:00:00",
        "sleep": 10,
        "childItems": true
    }
}

```


## <a name="type-properties"></a>Propiedades de tipo

Propiedad | DESCRIPCIÓN | Valores permitidos | Obligatorio
-------- | ----------- | -------------- | --------
Nombre | Nombre de la actividad "Validación" | string | Sí |
Tipo | Debe establecerse en **validación**. | string | Sí |
dataset | Actividad va a bloquear la ejecución hasta que ha validado esta referencia de conjunto de datos existe y que cumple los criterios especificados, o se ha alcanzado el tiempo de espera. Conjunto de datos proporcionado debe admitir la propiedad "MinimumSize" o "ChildItems". | Referencia de conjunto de datos | Sí |
timeout | Especifica el tiempo de espera para que se ejecute la actividad. Si se especifica ningún valor, el valor predeterminado es 7 días ("7.00:00:00"). El formato es d.hh:mm:ss | string | Sin  |
Modo de suspensión | Un retardo en segundos entre los intentos de validación. Si se especifica ningún valor, el valor predeterminado es 10 segundos. | Entero | Sin  |
childItems | Comprueba si la carpeta tiene elementos secundarios. Se puede establecer en true: Validar que la carpeta existe y que tiene elementos. Bloquea hasta que al menos un elemento está presente en la carpeta o se alcanza el valor de tiempo de espera.-false: Validar que la carpeta existe y que está vacía. Se bloquea hasta que la carpeta está vacía o hasta que el tiempo de espera se alcanza el valor. Si se especifica ningún valor, actividad se bloqueará hasta que exista la carpeta o hasta que se alcanza el tiempo de espera. | Boolean | Sin  |
minimumSize | Tamaño mínimo de un archivo en bytes. Si se especifica ningún valor, valor predeterminado es de 0 bytes | Entero | Sin  |


## <a name="next-steps"></a>Pasos siguientes
Consulte otras actividades de flujo de control compatibles con Data Factory:

- [Actividad If Condition](control-flow-if-condition-activity.md)
- [Actividad de ejecución de canalización](control-flow-execute-pipeline-activity.md)
- [Para cada actividad](control-flow-for-each-activity.md)
- [Actividad de obtención de metadatos](control-flow-get-metadata-activity.md)
- [Actividad Lookup](control-flow-lookup-activity.md)
- [Actividad web](control-flow-web-activity.md)
- [Actividad Until](control-flow-until-activity.md)
