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
ms.date: 01/17/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: a1f5a698ee76ebd0561bd19ff1a23d0f04be0771
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2019
ms.locfileid: "54410122"
---
#   <a name="shaper-cognitive-skill"></a>Habilidad cognitiva Conformador

La habilidad **Conformador** crea un tipo complejo para admitir campos compuestos (también conocidos como campos de varias partes). Un campo de tipo complejo consta de varias partes, pero se trata como un elemento único en un índice de Azure Search. Los ejemplos de campos consolidados útiles en escenarios de búsqueda incluyen la combinación de un nombre y apellido en un solo campo, de la ciudad y el estado en un solo campo o del nombre y la fecha de nacimiento en un único campo para establecer la identidad exclusiva.

La aptitud **Conformador** básicamente permite crear una estructura, definir el nombre de los miembros de esa estructura y asignar valores a cada miembro.

De forma predeterminada, esta técnica admite objetos de un nivel de profundidad. Para objetos más complejos, puede encadenar varios pasos de **Conformador**.

En la respuesta, el nombre de la salida es siempre "output". Internamente, la canalización puede asignar otro nombre a "output" (por ejemplo, "analyzedText" en los ejemplos siguientes), pero la aptitud **Conformador** devuelve "output" en la respuesta. Esto podría ser importante si está depurando documentos enriquecidos y observa la discrepancia de nombre, o si compila una habilidad personalizada y estructura la respuesta por sí mismo.

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
      "targetName": analyzedText"
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
            "output": "titlesAndChapters",
            "targetName": "analyzedText"
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

## <a name="see-also"></a>Otras referencias

+ [Aptitudes predefinidas](cognitive-search-predefined-skills.md)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)

