---
title: 'Aptitud de búsqueda cognitiva Conformador: Azure Search'
description: Extraiga metadatos e información estructurada a partir de datos no estructurados y confórmelos como un tipo complejo en una canalización de enriquecimiento de Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: c55783e9b209a1280a21edca34b75e72481f4cb6
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2019
ms.locfileid: "56807004"
---
#   <a name="shaper-cognitive-skill"></a>Habilidad cognitiva Conformador

El **conformador** aptitud consolida varias entradas en un tipo complejo que se puede hacer referencia más adelante en la canalización de enriquecimiento. La aptitud **Conformador** básicamente permite crear una estructura, definir el nombre de los miembros de esa estructura y asignar valores a cada miembro. Algunos ejemplos de campos consolidados útiles en escenarios de búsqueda son la combinación de un nombre y apellido en una única estructura, ciudad y estado en una única estructura o el nombre y fecha de nacimiento en una única estructura para establecer la identidad única.

De forma predeterminada, esta técnica admite objetos de un nivel de profundidad. Para objetos más complejos, puede encadenar varios pasos de **Conformador**.

En la respuesta, el nombre de la salida es siempre "salida". Internamente, la canalización puede asignar otro nombre a "output" (por ejemplo, "analyzedText" en los ejemplos siguientes), pero la aptitud **Conformador** devuelve "output" en la respuesta. Esto podría ser importante si está depurando documentos enriquecidos y observa la discrepancia de nombre, o si compila una habilidad personalizada y estructura la respuesta por sí mismo.

> [!NOTE]
> Esta aptitud no está enlazada a una API de Cognitive Services y no se le cobrará por usarla. Sin embargo, debe [adjuntar un recurso de Cognitive Services](cognitive-search-attach-cognitive-services.md) para invalidar la opción del recurso **Gratis**, que tiene un límite de unos pocos enriquecimientos al día.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ShaperSkill

## <a name="sample-1-complex-types"></a>Ejemplo 1: tipos complejos

Piense en un escenario en el que quiere crear una estructura denominada *analyzedText* con dos miembros: *text* y *sentiment*, respectivamente. En Azure Search, un campo de búsqueda de varias partes se denomina *tipo complejo* y aún no se admite desde el principio. En esta versión preliminar, se puede usar la aptitud **Conformador** para generar campos de tipo complejo en el índice. 

En el ejemplo siguiente se proporcionan los nombres de los miembros como entrada. La estructura de salida (el campo complejo en Azure Search) se especifica a través de *targetName*. 


```json
{
  "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
  "context": "/document/content/phrases/*",
  "inputs": [
    {
      "name": "text",
      "source": "/document/content/phrases/*"
    },
    {
      "name": "sentiment",
      "source": "/document/content/phrases/*/sentiment"
    }
  ],
  "outputs": [
    {
      "name": "output",
      "targetName": "analyzedText"
    }
  ]
}
```

### <a name="sample-input"></a>Entrada de ejemplo
Un documento JSON que proporciona datos de entrada útiles para esta aptitud **Conformador** podría ser:

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "this movie is awesome",
                "sentiment": 0.9
            }
        }
    ]
}
```


### <a name="sample-output"></a>Salida de ejemplo
La aptitud *Conformador* genera un nuevo elemento llamado **analyzedText** con los elementos combinados de *text* y *sentiment*. 

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
            "analyzedText": 
              {
                "text": "this movie is awesome" ,
                "sentiment": 0.9
              }
           }
      }
    ]
}
```

## <a name="sample-2-input-consolidation"></a>Ejemplo 2: consolidación de entrada

En otro ejemplo, imagine que en distintas fases del procesamiento de la canalización ha extraído el título de un libro y los títulos de capítulos de distintas páginas del libro. Ahora podría crear una única estructura compuesta por estas distintas entradas.

La definición de la habilidad Conformador de este escenario podría ser similar al ejemplo siguiente:

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "context": "/document",
    "inputs": [
        {
            "name": "title",
            "source": "/document/content/title"
        },
        {
            "name": "chapterTitles",
            "source": "/document/content/pages/*/chapterTitles/*"
        }
    ],
    "outputs": [
        {
            "name": "output",
            "targetName": "titlesAndChapters"
        }
    ]
}
```

### <a name="sample-output"></a>Salida de ejemplo
En este caso, Conformador acopla todos los títulos de capítulos para crear una matriz única. 

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "titlesAndChapters": {
                    "title": "How to be happy",
                    "chapterTitles": [
                        "Start young",
                        "Laugh often",
                        "Eat, sleep and exercise"
                    ]
                }
            }
        }
    ]
}
```

## <a name="see-also"></a>Vea también

+ [Aptitudes predefinidas](cognitive-search-predefined-skills.md)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)

