---
title: ¿Qué es Project Answer Search?
titlesuffix: Azure Cognitive Services
description: Introducción a Project Answer Search.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: overview
ms.date: 04/13/2018
ms.author: rosh
ms.openlocfilehash: ac1717a8e8a08fcfedc3bc21bb0f03b3e3ca2511
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "60721870"
---
# <a name="what-is-project-answer-search"></a>¿Qué es Project Answer Search?
Project Answer Search API usa el punto de conexión Bing v7 para obtener respuestas a preguntas. Una pregunta como "What is the circumference of the earth?" (¿Cuál es la circunferencia de la Tierra?) devuelve una respuesta con información objetiva.  Una consulta sobre una persona, un lugar o una cosa devuelve información sobre la entidad identificada por la consulta. Estos escenarios pueden resultar útiles en aplicaciones como bots de conversaciones, aplicaciones de mensajería, lectores, etc.  

Las consultas devuelven respuestas que dependen del escenario de consulta: siempre se devuelven páginas web, mientras que los [hechos](fact-queries.md) o las [entidades](entity-queries.md) se devuelven si resulta pertinente.

## <a name="endpoint"></a>Punto de conexión
Para obtener respuestas a una pregunta o información sobre una persona, un lugar o una cosa, envíe una solicitud al punto de conexión de Answer Search API. Use los encabezados y parámetros de dirección URL para varias especificaciones.  Incluya el encabezado *Ocp-Apim-Subscription-Key* con un token válido.  El parámetro market es obligatorio. Solo se admite el parámetro market `en-us` actualmente.

La consulta siguiente obtiene respuestas a la pregunta: "What is the circumference of the earth?" (¿Cuál es la circunferencia de la Tierra?)

GET:
```
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=what+is+circumference+of+the=earth?&mkt=en-us

```

El parámetro de dirección URL `q=` es obligatorio para especificar el objeto de búsqueda.

## <a name="response-object"></a>Objeto de respuesta

La respuesta incluye encabezados HTTP, páginas web, hechos o entidades.

```
BingAPIs-TraceId: AB2E75C998614ADB8EBF5110DF648298
X-MSEdge-ClientID: 1E48FC4F7B8768C80B14F7997A106906
BingAPIs-SessionId: 0504DDD6DAE84861A4842306F8DA7A58
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: AB2E75C998614ADB8EBF5110DF648298 Ref B: CO1EDGE0322 Ref C: 2018-04-19T19:57:13Z

JSON Response:

{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "what is the circumference of earth"
  },
  "webPages": {
    "webSearchUrl": "https://www.bing.com/search?q\u003dwhat+is+the+circumference+of+earth",
    "totalEstimatedMatches": 217000,
    "value": [
      {
        "id": "https://www.bingapis.com/api/v7/#WebPages.0",
        "name": "Circumference of the Earth - Universe Today",
        "url": "https://www.universetoday.com/26461/circumference-of-the-earth/",
        "isFamilyFriendly": true,
        "displayUrl": "https://www.universetoday.com/26461/circumference-of-the-earth",
        "snippet": "The circumference of the Earth in kilometers is 40,075 km, and the circumference of the Earth in miles is 24,901. In other words, if you could drive your car around the equator of the Earth (yes, even over the oceans), youâ€™d put on an extra 40,075 km on the odometer.",
        "deepLinks": [
          {
            "name": "About Earth",
            "url": "https://www.universetoday.com/14382/10-interesting-facts-about-planet-earth/"
          }
        ],
        "dateLastCrawled": "2018-04-12T14:13:00.0000000Z",
        "language": "en"
      },
      {
        "id": "https://www.bingapis.com/api/v7/#WebPages.1",
        "name": "Earth - Wikipedia",
        "url": "https://en.wikipedia.org/wiki/Earth",
        "about": [
          {
            "name": "Earth"
          },
          {
            "name": "Earth"
          }
        ],
        "isFamilyFriendly": true,
        "displayUrl": "https://en.wikipedia.org/wiki/Earth",
        "snippet": "Circumference: 40 075.017 km equatorial (24 901.461 mi) ... Earth is the third planet from the Sun and the only object in the Universe known to harbor life.",
        "deepLinks": [
          {
            "name": "Moon",
            "url": "https://en.wikipedia.org/wiki/Moon"
          },
          {
            "name": "Planet",
            "url": "https://en.wikipedia.org/wiki/Planet"
          },
          {
            "name": "Quasi-Satellites",
            "url": "https://en.wikipedia.org/wiki/Quasi-satellite"
          },
          {
            "name": "World Population",
            "url": "https://en.wikipedia.org/wiki/World_population"
          },
   . . .

    ]
  },
  "entities": {
    "value": [
      {
        "id": "https://www.bingapis.com/api/v7/#Entities.0",
        "contractualRules": [
          {
            "_type": "ContractualRules/LicenseAttribution",
            "targetPropertyName": "description",
            "mustBeCloseToContent": true,
            "license": {
              "name": "CC-BY-SA",
              "url": "https://creativecommons.org/licenses/by-sa/3.0/"
            },
            "licenseNotice": "Text under CC-BY-SA license"
          },
          {
            "_type": "ContractualRules/LinkAttribution",
            "targetPropertyName": "description",
            "mustBeCloseToContent": true,
            "text": "Wikipedia",
            "url": "https://en.wikipedia.org/wiki/Earth"
          },
          {
            "_type": "ContractualRules/MediaAttribution",
            "targetPropertyName": "image",
            "mustBeCloseToContent": true,
            "url": "https://en.wikipedia.org/wiki/Earth"
          }
        ],
        "webSearchUrl": "https://www.bing.com/entityexplore?q\u003dEarth\u0026filters\u003dsid:%226ddb3372-4801-5567-321e-e8a53bd774a4%22\u0026elv\u003dAXXfrEiqqD9r3GuelwApulpmymQx!ODfuQu*veOQHkvP0!Zbvi5F5tVcMSDJvDEWiQWwrdueYTtIszgj03oFQHykYYLYgq3q5!Sf00QxXGIS",
        "name": "Earth",
        "image": {
          "name": "Earth",
          "thumbnailUrl": "https://www.bing.com/th?id\u003dA3ab623665ab412f386c162bd29f0683a\u0026w\u003d110\u0026h\u003d110\u0026c\u003d7\u0026rs\u003d1\u0026qlt\u003d80\u0026cdv\u003d1\u0026pid\u003d16.1",
          "provider": [
            {
              "_type": "Organization",
              "url": "https://en.wikipedia.org/wiki/Earth"
            }
          ],
          "hostPageUrl": "https://upload.wikimedia.org/wikipedia/commons/9/97/The_Earth_seen_from_Apollo_17.jpg",
          "width": 110,
          "height": 110,
          "sourceWidth": 799,
          "sourceHeight": 800
        },
        "description": "Earth is the third planet from the Sun and the only object in the Universe known to harbor life. According to radiometric dating and other sources of evidence, Earth formed over 4.5 billion years ago. Earth\u0027s gravity interacts with other objects in space, especially the Sun and the Moon, Earth\u0027s only natural satellite. Earth revolves around the Sun in 365.26 days, a period known as an Earth year. During this time, Earth rotates about its axis about 366.26 times.",
        "entityPresentationInfo": {
          "entityScenario": "DominantEntity",
          "entityTypeHints": [
            "Generic"
          ]
        },
        "bingId": "6ddb3372-4801-5567-321e-e8a53bd774a4"
      }
    ]
  },
  "facts": {
    "id": "https://www.bingapis.com/api/v7/#Facts",
    "contractualRules": [
      {
        "_type": "ContractualRules/LinkAttribution",
        "text": "www.universetoday.com/26461/circumference-of-the-earth/",
        "url": "http://www.universetoday.com/26461/circumference-of-the-earth/"
      }
    ],
    "attributions": [
      {
        "providerDisplayName": "www.universetoday.com/26461/circumference-of-the-earth/",
        "seeMoreUrl": "http://www.universetoday.com/26461/circumference-of-the-earth/"
      }
    ],
    "value": [
      {
        "description": "The circumference of the Earth in kilometers is 40,075 km, and the circumference of the Earth in miles is 24,901. In other words, if you could drive your car around the equator of the Earth (yes, even over the oceans), youâ€™d put on an extra 40,075 km on the odometer.",
        "subjectName": ""
      }
    ]
  },
  "rankingResponse": {
    "mainline": {
      "items": [
        {
          "answerType": "Facts",
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Facts"
          }
        },
        {
          "answerType": "WebPages",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#WebPages.0"
          }
        },
        {
          "answerType": "WebPages",
          "resultIndex": 1,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#WebPages.1"
          }
        },
        {
          "answerType": "WebPages",
          "resultIndex": 2,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#WebPages.2"
          }
        },
        
        . . . 
      ]
    },
    "sidebar": {
      "items": [
        {
          "answerType": "Entities",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.0"
          }
        }
      ]
    }
  }
}

```

## <a name="terms-of-use"></a>Términos de uso
Project Answer Search y Project Video Trends están sujetas a los [requisitos de uso y visualización de Bing Search](use-display-requirements.md).

Tenga en cuenta que ni usted, ni un tercero en su nombre, pueden usar, retener, guardar, almacenar en caché, compartir o distribuir datos de URL Preview API con el fin de probar, desarrollar, entrenar, distribuir o ponerlos a disposición cualquier servicio o característica que no sea de Microsoft. 

## <a name="throttling-requests"></a>Solicitudes de limitación

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]


## <a name="data-attribution"></a>Atribución de datos  

Las respuestas de Project Answer Search contienen información que es propiedad de terceros. Usted es responsable de garantizar que su uso sea apropiado; por ejemplo, debe cumplir con cualquier licencia de Creative Commons en la que se base su experiencia de usuario.  
  
Si una respuesta o resultado incluye los campos `contractualRules`, `attributions` o `provider`, debe atribuir los datos. Si la respuesta no incluye ninguno de estos campos, no es necesario realizar la atribución. Si la respuesta incluye el campo `contractualRules` y los campos `attributions` o `provider`, debe usar las reglas contractuales para atribuir los datos.  
  
En el siguiente ejemplo se muestra una entidad que incluye una regla contractual MediaAttribution y una imagen que incluye un campo `provider`. La regla MediaAttribution identifica la imagen como el destino de la regla, por lo que debe ignorar el campo `provider` de la imagen y, en su lugar, usar la regla MediaAttribution para proporcionar la atribución.  
  
```  
        "value" : [{
            "contractualRules" : [
                . . .
                {
                    "_type" : "ContractualRules\/MediaAttribution",
                    "targetPropertyName" : "image",
                    "mustBeCloseToContent" : true,
                    "url" : "http:\/\/en.wikipedia.org\/wiki\/Space_Needle"
                }
            ],
            . . .
            "image" : {
                "name" : "Space Needle",
                "thumbnailUrl" : "https:\/\/www.bing.com\/th?id=A46378861201...",
                "provider" : [{
                    "_type" : "Organization",
                    "url" : "http:\/\/en.wikipedia.org\/wiki\/Space_Needle"
                }],
                "hostPageUrl" : "http:\/\/www.citydictionary.com\/Uploaded...",
                "width" : 110,
                "height" : 110
            },
            . . .
        }]
```  
  
Si una regla contractual incluye el campo `targetPropertyName`, la regla solo se aplica al campo de destino. De lo contrario, la regla se aplica al objeto principal que contiene el campo `contractualRules`.  
  
  
En el siguiente ejemplo, la regla `LinkAttribution` incluye el campo `targetPropertyName`, por lo que la regla se aplica al campo `description`. En cuanto a las reglas que se aplican a campos específicos, debe incluir una línea inmediatamente después de los datos de destino que contienen un hipervínculo al sitio web del proveedor. Por ejemplo, para atribuir la descripción, incluya una línea inmediatamente después del texto descriptivo que contiene un hipervínculo a los datos del sitio web del proveedor; en este caso, debe crear un vínculo a es.wikipedia.org.  
  
```  
"entities" : {  
    "value" : [{  
            . . .  
            "description" : "Peyton Williams Manning is a former American....",  
            . . .  
            "contractualRules" : [{  
                    "_type" : "ContractualRules\/LinkAttribution",  
                    "targetPropertyName" : "description",  
                    "mustBeCloseToContent" : true,  
                    "text" : "en.wikipedia.org",  
                    "url" : "http:\/\/www.bing.com\/cr?IG=B8AD73..."  
                 },  
            . . .  
  
```  

### <a name="license-attribution"></a>Atribución de licencias  

Si la lista de reglas contractuales incluye una regla [LicenseAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#licenseattribution), debe mostrar el aviso en la línea inmediatamente después del contenido al que se aplica la licencia. La regla `LicenseAttribution` usa el campo `targetPropertyName` para identificar la propiedad a la que se aplica la licencia.  
  
A continuación, se muestra un ejemplo que incluye una regla `LicenseAttribution`.  
  
![Atribución de licencias](./media/licenseattribution.png)  
  
El aviso de licencia que muestre debe incluir un hipervínculo al sitio web que contiene información sobre la licencia. Por lo general, debe convertir el nombre de la licencia en un hipervínculo. Por ejemplo, si el aviso es **Texto con licencia CC-BY-SA** y CC-BY-SA es el nombre de la licencia, debe hacer que CC-BY-SA sea un hipervínculo.  
  
### <a name="link-and-text-attribution"></a>Atribución de vínculo y texto  

Las reglas [LinkAttribution](reference.md#linkattribution) y [TextAttribution](reference.md#textattribution) se usan generalmente para identificar al proveedor de los datos. El campo `targetPropertyName` identifica el campo al que se aplica la regla.  
  
Para atribuir los proveedores, incluya una línea inmediatamente después del contenido al que se aplican las atribuciones (por ejemplo, el campo de destino). La línea debe estar claramente etiquetada para indicar que los proveedores son la fuente de los datos. Por ejemplo, "Datos de: es.wikipedia.org". En cuanto a las reglas `LinkAttribution`, debe crear un hipervínculo al sitio web del proveedor.  
  
A continuación, se muestra un ejemplo que incluye las reglas `LinkAttribution` y `TextAttribution`.  
  
![Atribución de texto del vínculo](./media/linktextattribution.png)  

### <a name="media-attribution"></a>Atribución de medios  

Si la entidad incluye una imagen y la visualiza, debe proporcionar un vínculo para obtener acceso al sitio web del proveedor con un simple clic. Si la entidad incluye una regla [MediaAttribution](reference.md#mediaattribution), use la dirección URL de la regla para crear el vínculo al cual obtener acceso haciendo clic. De lo contrario, use la dirección URL incluida en el campo `provider` de la imagen para crear el vínculo al que se puede acceder con un solo clic.  
  
A continuación, se muestra un ejemplo que incluye el campo `provider` de una imagen y las reglas contractuales. Como el ejemplo incluye la regla contractual, debe ignorar el campo `provider` de la imagen y aplicar la regla `MediaAttribution`.  
  
![Atribución de medios](./media/mediaattribution.png)  

## <a name="next-steps"></a>Pasos siguientes
- [Inicio rápido de C#](c-sharp-quickstart.md)
- [Inicio rápido de Java](java-quickstart.md)
- [Inicio rápido de Node](node-quickstart.md)
- [Inicio rápido de Python](python-quickstart.md)
