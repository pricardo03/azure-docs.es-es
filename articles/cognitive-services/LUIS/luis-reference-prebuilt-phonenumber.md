---
title: 'Entidades precompiladas Phonenumber: LUIS'
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información de la entidad precompilada de número de teléfono en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 1cc7469bf6b29ed864fac3955dc8770aa879f84d
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499523"
---
# <a name="phone-number-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada de número de teléfono para una aplicación de LUIS
La entidad `phonenumber` extrae una variedad de números de teléfono, incluido el código de país. Dado que esta entidad ya está entrenada, no se necesita agregar expresiones de ejemplo que a la aplicación. La entidad `phonenumber` se admite solamente en la referencia cultural `en-us`. 

## <a name="types-of-a-phone-number"></a>Tipos de un número de teléfono
`Phonenumber` se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml).

## <a name="resolution-for-this-prebuilt-entity"></a>Resolución para esta entidad precompilada

Se devuelven los siguientes objetos entidad para la consulta:

`my mobile is 1 (800) 642-7676`

#### <a name="v3-responsetabv3"></a>[Respuesta de V3](#tab/V3)

El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
"entities": {
    "phonenumber": [
        "1 (800) 642-7676"
    ]
}
```
#### <a name="v3-verbose-responsetabv3-verbose"></a>[Respuesta detallada de V3](#tab/V3-verbose)
El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
"entities": {
    "phonenumber": [
        "1 (800) 642-7676"
    ],
    "$instance": {

        "phonenumber": [
            {
                "type": "builtin.phonenumber",
                "text": "1 (800) 642-7676",
                "startIndex": 13,
                "length": 16,
                "score": 1.0,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```
#### <a name="v2-responsetabv2"></a>[Respuesta de V2](#tab/V2)

En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.phonenumber**.

```json
"entities": [
    {
        "entity": "1 (800) 642-7676",
        "type": "builtin.phonenumber",
        "startIndex": 13,
        "endIndex": 28,
        "resolution": {
            "score": "1",
            "value": "1 (800) 642-7676"
        }
    }
]
```
* * * 

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

Obtenga información acerca de las entidades de [porcentaje](luis-reference-prebuilt-percentage.md), [número de teléfono](luis-reference-prebuilt-number.md) y [temperatura](luis-reference-prebuilt-temperature.md). 
