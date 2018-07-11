---
title: Cómo usar la vinculación de entidad en la API de REST de Text Analytics (Microsoft Cognitive Services en Azure) | Microsoft Docs
description: En este tutorial encontrará información acerca de cómo identificar y resolver entidades mediante la API de REST de Text Analytics en Microsoft Cognitive Services en Azure.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: article
ms.date: 5/02/2018
ms.author: ashmaka
ms.openlocfilehash: 55bec1a0223b70749a97a30e2da92ef15128038c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380899"
---
# <a name="how-to-identify-linked-entities-in-text-analytics-preview"></a>Cómo identificar entidades vinculadas en Text Analytics (versión preliminar)

[Entity Linking API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) toma el texto no estructurado y, para cada documento JSON, devuelve una lista de entidades desambiguadas con vínculos a más información en la web (Wikipedia y Bing). 

## <a name="entity-linking-vs-named-entity-recognition"></a>Vinculación de entidad frente a Reconocimiento de entidades con nombre

En el procesamiento de lenguaje natural, los conceptos de vinculación de entidad y reconocimiento de entidades con nombre (NER) pueden confundirse fácilmente. En la versión preliminar del punto de conexión `entities` de Text Analytics, solo se admite la vinculación de entidad.

La vinculación de entidad es la capacidad de identificar y desambiguar la identidad de una entidad que se encuentra en el texto (por ejemplo, determinar si se está usando "Marte" como el planeta o como el dios romano de la guerra). Este proceso requiere la presencia de una knowledge base a la que se vinculan las entidades reconocidas (Wikipedia se usa como knowledge base del punto de conexión `entities` de Text Analytics).

### <a name="language-support"></a>Compatibilidad con idiomas

El uso de la vinculación de entidad en distintos idiomas requiere una knowledge base correspondiente en cada idioma. Para la vinculación de entidad en Text Analytics, esto significa que cada idioma compatible con el punto de conexión `entities` se vinculará al corpus de Wikipedia correspondiente en dicho idioma. Dado que el tamaño del corpus varía según el idioma, se espera que la coincidencia de la funcionalidad de vinculación de entidad también varíe.


## <a name="preparation"></a>Preparación

Debe tener documentos JSON en este formato: identificador, texto, idioma

Para conocer los idiomas admitidos actualmente, consulte [esta lista](../text-analytics-supported-languages.md).

El tamaño del documento debe ser inferior a 5000 caracteres por documento y puede tener hasta 1000 elementos (ID) por colección. La colección se envía en el cuerpo de la solicitud. El ejemplo siguiente es una ilustración del contenido que podría enviar al extremo de la vinculación de entidad.

```
{"documents": [{"id": "1",
                "language": "en",
                "text": "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."
                },
               {"id": "2",
                "language": "en",
                "text": "The Seattle Seahawks won the Super Bowl in 2014."
                }
               ]
}
```    
    
## <a name="step-1-structure-the-request"></a>Paso 1: Estructurar la solicitud

Se pueden encontrar detalles sobre la definición de la solicitud en [How to call the Text Analytics API](text-analytics-how-to-call-api.md) (Cómo llamar a Text Analytics API). Los siguientes puntos se replantean para su comodidad:

+ Cree una solicitud **POST**. Revise la documentación de la API para esta solicitud: [Entity Linking API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634).

+ Establezca el punto de conexión HTTP para la extracción de frases clave. Debe incluir el recursos`/entities`: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/entities`.

+ Establezca un encabezado de solicitud para incluir la clave de acceso para las operaciones de Text Analytics. Para obtener más información, consulte [How to find endpoints and access keys](text-analytics-how-to-access-key.md) (Cómo buscar puntos de conexión y claves de acceso).

+ En el cuerpo de la solicitud, proporcione la colección de documentos JSON que preparó para este análisis.

> [!Tip]
> Use [Postman](text-analytics-how-to-call-api.md) o abra la **consola de prueba de la API** en la [documentación](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) para estructurar y enviar una solicitud POST al servicio.

## <a name="step-2-post-the-request"></a>Paso 2: Enviar la solicitud

El análisis se realiza tras la recepción de la solicitud. El servicio acepta hasta 100 solicitudes por minuto. Cada solicitud puede tener un máximo de 1 MB.

Recuerde que el servicio no tiene estado. No se almacena ningún dato en su cuenta. Los resultados se devuelven inmediatamente en la respuesta.

## <a name="step-3-view-results"></a>Paso 3: Ver los resultados

Todas las solicitudes POST devolverán una respuesta con formato JSON con los identificadores y las propiedades detectadas.

La salida se devuelve inmediatamente. Puede transmitir los resultados a una aplicación que acepte JSON o guardar la salida en un archivo en el sistema local y, a continuación, importarlo en una aplicación que permita ordenar, buscar y manipular los datos.

A continuación se muestra un ejemplo de la salida de la vinculación de entidad:

```
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "name": "Xbox One",
                    "matches": [
                        {
                            "text": "XBox One",
                            "offset": 23,
                            "length": 8
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "Xbox One",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/Xbox_One",
                    "bingId": "446bb4df-4999-4243-84c0-74e0f6c60e75"
                },
                {
                    "name": "Ultra-high-definition television",
                    "matches": [
                        {
                            "text": "4K",
                            "offset": 63,
                            "length": 2
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "Ultra-high-definition television",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/Ultra-high-definition_television",
                    "bingId": "7ee02026-b6ec-878b-f4de-f0bc7b0ab8c4"
                }
            ]
        },
        {
            "id": "2",
            "entities": [
                {
                    "name": "2013 Seattle Seahawks season",
                    "matches": [
                        {
                            "text": "Seattle Seahawks",
                            "offset": 4,
                            "length": 16
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "2013 Seattle Seahawks season",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/2013_Seattle_Seahawks_season",
                    "bingId": "eb637865-4722-4eca-be9e-0ac0c376d361"
                }
            ]
        }
    ],
    "errors": []
}
```

Si está disponible, la respuesta incluye el id. de Wikipedia, la dirección URL de Wikipedia y el id. de Bing de cada entidad detectada. Estos datos se pueden usar para mejorar aún más la aplicación con información relativa a la entidad vinculada.


## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y el flujo de trabajo de vinculación de entidad mediante Text Analytics de Cognitive Services. En resumen:

+ [Entity Linking API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) está disponible para los idiomas seleccionados.
+ Los documentos JSON del cuerpo de la solicitud incluyen un id., texto y el código de idioma.
+ La solicitud POST se realiza a un punto de conexión `/entities`, con una [clave de acceso y un punto de conexión](text-analytics-how-to-access-key.md) personalizados, que son válidos para la suscripción.
+ La salida de respuesta, que consta de entidades vinculadas (incluidas las puntuaciones de confianza, los desplazamientos y los vínculos web de cada id. de documento) se puede usar en cualquier aplicación.

## <a name="see-also"></a>Otras referencias 

 [Información general de Text Analytics](../overview.md)  
 [Preguntas más frecuentes (P+F)](../text-analytics-resource-faq.md)</br>
 [Página del producto de Text Analytics](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Text Analytics API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)
