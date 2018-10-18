---
title: 'Detectar contenido específico del dominio: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la descripción de imágenes mediante Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: a9c71fa7e5d86cfeb4fe6fab44bbce241546ccb8
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342573"
---
# <a name="detecting-domain-specific-content"></a>Detección de contenido específico del dominio

Además de las etiquetas y las categorías de nivel superior, Computer Vision también admite información especializada (o específica del dominio). La información especializada se puede implementar como un método independiente o con la categorización de alto nivel. Funciona como un método para mejorar la taxonomía de las 86 categorías a través de la adición de modelos específicos del dominio.

Hay dos opciones para usar los modelos específicos del dominio:

* Análisis con ámbito  
  Se analiza solo un modelo elegido, mediante la invocación de una llamada HTTP POST. Si sabe qué modelo usar, especifique el nombre del mismo. Solo obtendrá información pertinente para ese modelo. Por ejemplo, puede usar esta opción para el reconocimiento de celebridades. La respuesta contiene una lista de potenciales coincidencias de celebridades, acompañadas de su puntuación de confianza.
* Análisis mejorado  
  Análisis que proporciona detalles adicionales relacionados con las categorías de la taxonomía de las 86 categorías. Esta opción está disponible para aplicaciones donde los usuarios quieren obtener un análisis genérico de imágenes adicional a los detalles de uno o más modelos específicos del dominio. Cuando se llama a este método, se llama primero al clasificador de la taxonomía de las 86 categorías. Si cualquiera de las categorías concuerda con los modelos conocidos o coincidentes, se procede a una segunda pasada de invocaciones de clasificador. Por ejemplo, si el parámetro `details` de la llamada HTTP POST se establece en "all" o si incluye "celebrities", el método llama al clasificador de celebridades una vez llamado el clasificador de las 86 categorías. Si la imagen está clasificada como `people_` o hace referencia a una subcategoría de esa categoría, se llama al clasificador de celebridades.

## <a name="listing-domain-specific-models"></a>Enumerar modelos específicos de dominio

Puede enumerar los modelos específicos de dominio que sean compatibles con Computer Vision. Actualmente, Computer Vision admite los siguientes modelos específicos de dominio para detectar contenido específico del mismo:

| NOMBRE | DESCRIPCIÓN |
|------|-------------|
| celebrities | Reconocimiento de celebridades, compatible con imágenes clasificadas en la categoría `people_`. |
| landmarks | Reconocimiento de puntos de referencia, compatible con imágenes clasificadas en las categorías `outdoor_` o `building_`. |

### <a name="domain-model-list-example"></a>Ejemplo de lista del modelo de dominio

La siguiente respuesta JSON puede enumerar los modelos específicos de dominio que sean compatibles con Computer Vision.

```json
{
    "models": [
        {
            "name": "celebrities",
            "categories": ["people_", "人_", "pessoas_", "gente_"]
        },
        {
            "name": "landmarks",
            "categories": ["outdoor_", "户外_", "屋外_", "aoarlivre_", "alairelibre_",
                "building_", "建筑_", "建物_", "edifício_"]
        }
    ]
}
```

## <a name="next-steps"></a>Pasos siguientes

Conozca los conceptos de [categorización de imágenes](concept-categorizing-images.md).