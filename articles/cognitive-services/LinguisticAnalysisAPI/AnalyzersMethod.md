---
title: 'Método de analizadores: Lingistic Analysis API'
titlesuffix: Azure Cognitive Services
description: La API REST de analizadores proporciona una lista de los analizadores que son actualmente compatibles con Linguistic Analysis API.
services: cognitive-services
author: RichardSunMS
manager: cgronlun
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: conceptual
ms.date: 06/30/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: 762ebf50999a88251dcd05824f2ed450cec97f04
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237406"
---
# <a name="analyzers-method"></a>Método de analizadores

> [!IMPORTANT]
> El 9 de agosto de 2018 se retiró la versión preliminar de Linguistic Analysis. Para el procesamiento y el análisis de texto, se recomienda usar los [módulos de análisis de texto de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics).

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
