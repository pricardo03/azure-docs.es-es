---
title: 'Entidad precompilada PersonName: LUIS'
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información sobre la entidad pregenerada personName en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 9777c62d97c70d4f6a0d0a4d912dea3fa8decd23
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499551"
---
# <a name="personname-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada PersonName para una aplicación de LUIS
La entidad pregenerada personName detecta los nombres de personas. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan la entidad personName para los propósitos de la aplicación. La entidad personName se admite en las [referencias culturales](luis-reference-prebuilt-entities.md) de inglés y chino.

## <a name="resolution-for-personname-entity"></a>Resolución de la entidad personName

Se devuelven los siguientes objetos entidad para la consulta:

`Is Jill Jones in Cairo?`


#### <a name="v3-responsetabv3"></a>[Respuesta de V3](#tab/V3)


El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
"entities": {
    "personName": [
        "Jill Jones"
    ]
}
```
#### <a name="v3-verbose-responsetabv3-verbose"></a>[Respuesta detallada de V3](#tab/V3-verbose)
El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
"entities": {
    "personName": [
        "Jill Jones"
    ],
    "$instance": {
        "personName": [
            {
                "type": "builtin.personName",
                "text": "Jill Jones",
                "startIndex": 3,
                "length": 10,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
    }
}
```
#### <a name="v2-responsetabv2"></a>[Respuesta de V2](#tab/V2)

En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.personName**.

```json
"entities": [
{
    "entity": "Jill Jones",
    "type": "builtin.personName",
    "startIndex": 3,
    "endIndex": 12
}
]
```
* * * 

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

Obtenga información acerca de las entidades de [correo electrónico](luis-reference-prebuilt-email.md), [número](luis-reference-prebuilt-number.md) y [ordinal](luis-reference-prebuilt-ordinal.md). 
