---
title: Método de analizadores en Linguistic Analysis API | Microsoft Docs
description: La API REST de analizadores proporciona una lista de los analizadores que son actualmente compatibles con el servicio de Microsoft Cognitive Services.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 06/30/2016
ms.author: lesun
ms.openlocfilehash: 3fc243a0da77c5bae9009929f2b82e1353347752
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380066"
---
# <a name="analyzers-method"></a>Método de analizadores

La API REST de **analizadores** proporciona una lista de los analizadores actualmente compatibles con el servicio.
La respuesta incluye sus [nombres](Analyzer-Names.md) y los idiomas admitidos por cada uno de ellos (por ejemplo, "en" para inglés).

## <a name="request-parameters"></a>Parámetros de solicitud
None

<br>

## <a name="response-parameters"></a>Parámetros de respuesta
NOMBRE | Escriba | DESCRIPCIÓN
-----|------|--------------
languages | lista de cadenas | lista de códigos de idioma de dos letras ISO para los que se puede utilizar este analizador
id   | string | identificador único para este analizador
kind | string | tipo amplio de analizador aquí
specification | string | nombre de la especificación utilizada para este analizador
implementation | string | descripción del modelo y/o del algoritmo detrás de este analizador

<br>
## <a name="example"></a>Ejemplo
GET /analyzers

Respuesta: JSON
```json
[
    {
        "id": "22A6B758-420F-4745-8A3C-46835A67C0D2",
        "languages": ["en"],
        "kind": "Constituency_Tree",  
        "specification": "PennTreebank3", 
        "implementation": "SplitMerge"
    }, 
    {
        "id" : "4FA79AF1-F22C-408D-98BB-B7D7AEEF7F04",
        "languages": ["en"],
        "kind": "POS_Tags", 
        "specification": "PennTreebank3", 
        "implementation": "cmm"
    },
    {
        "id" : "08EA174B-BFDB-4E64-987E-602F85DA7F72",
        "languages": ["en"],
        "kind": "Tokens", 
        "specification":"PennTreebank3", 
        "implementation": "regexes"
    } 
]
```
