---
title: Uso del reconocimiento de entidades con Text Analytics API
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo identificar y eliminar la ambigüedad de la identidad de una entidad encontrada en el texto con la API REST de Text Analytics.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 11/21/2019
ms.author: aahi
ms.openlocfilehash: ae5222dcd05740ecb9747037b315c4e920b3eabd
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74326639"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics"></a>Uso del reconocimiento de entidades con nombre en Text Analytics

[Named Entity Recognition API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) toma texto no estructurado y, para cada documento JSON, devuelve una lista de entidades sin ambigüedad con vínculos a más información en la web (Wikipedia y Bing).

## <a name="entity-linking-and-named-entity-recognition"></a>Vinculación de entidad y reconocimiento de entidades con nombre

El punto de conexión `entities` de Text Analytics admite el reconocimiento de entidades con nombre (NER) y la vinculación de entidades.

### <a name="entity-linking"></a>Entity Linking
La vinculación de entidad es la capacidad de identificar y desambiguar la identidad de una entidad que se encuentra en el texto (por ejemplo, determinar si se está usando "Marte" como el planeta o como el dios romano de la guerra). Este proceso requiere la presencia de una knowledge base a la que se vinculan las entidades reconocidas (Wikipedia se usa como knowledge base del punto de conexión `entities` de Text Analytics).

### <a name="named-entity-recognition-ner"></a>Reconocimiento de entidades con nombre (NER)
El reconocimiento de entidades con nombre (NER) es la capacidad de identificar diferentes entidades en el texto y clasificarlas en clases predefinidas o tipos. 

## <a name="named-entity-recognition-v3-public-preview"></a>Versión preliminar pública de reconocimiento de entidades con nombre v3

La siguiente versión del reconocimiento de entidades con nombre ahora está disponible en versión preliminar pública. Proporciona actualizaciones para la vinculación de entidad y el reconocimiento de entidades con nombre. Pruebe a usar la [consola de prueba de API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesRecognitionGeneral).

:::row:::
    :::column span="":::
        **Característica**
    :::column-end:::
    ::: column span="":::
        **Descripción** 
    :::column-end:::
:::row-end:::
<!-- expanded types and subtypes row-->
:::row:::
    :::column span="":::
        Subtipos y tipos de entidad expandidos
    :::column-end:::
    :::column span="":::
     Clasificación y detección expandidas para varios tipos de entidad con nombre.
    :::column-end:::
:::row-end:::
<!-- separate endpoints row-->
:::row:::
    :::column span="":::
        Separación de los puntos de conexión independientes 
    :::column-end:::
    :::column span="":::
        Separe los puntos de conexión independientes para enviar las solicitudes de vinculación de entidad y de NER.
    :::column-end:::
:::row-end:::
<!-- model-version row -->
:::row:::
    :::column span="":::
        Parámetro `model-version`
    :::column-end:::
    :::column span="":::
        Un parámetro opcional para elegir una versión del modelo de Text Analytics. Actualmente, solo está disponible el modelo predeterminado para su uso.
    :::column-end:::
:::row-end:::

### <a name="entity-types"></a>Tipos de entidades

El reconocimiento de entidades con nombre v3 proporciona detección expandida en varios tipos. Actualmente, NER v3 puede reconocer las siguientes categorías de entidades. Para una lista detallada de las entidades y los lenguajes compatibles, consulte el artículo [Tipos de entidades con nombre](../named-entity-types.md).

* General
* Información personal 

### <a name="request-endpoints"></a>Puntos de conexión de solicitud

El reconocimiento de entidades con nombre v3 usa puntos de conexión independientes para las solicitudes de NER y de vinculación de entidad. Use un formato de dirección URL a continuación en función de la solicitud:

NER
* Entidades generales: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/general`

* Entidades de información personal: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/pii`

Vinculación de entidad
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/linking`

### <a name="model-versioning"></a>Control de versiones de los modelos

[!INCLUDE [v3-model-versioning](../includes/model-versioning.md)]

## <a name="supported-types-for-named-entity-recognition-v2"></a>Tipos admitidos para el reconocimiento de entidades con nombre v2

> [!NOTE]
> Las entidades siguientes son compatibles con la versión 2 del reconocimiento de entidades con nombre (NER). [NER v3](#named-entity-recognition-v3-public-preview) se encuentran en versión preliminar pública y expande en gran medida el número y la profundidad de las entidades que se reconocen en el texto.   

| type  | SubType | Ejemplo |
|:-----------   |:------------- |:---------|
| Persona        | N/D\*         | "Jeff", "Bill Gates"     |
| Location      | N/D\*         | "Redmond, Washington", "París"  |
| Organización  | N/D\*         | "Microsoft"   |
| Cantidad      | Number        | "6", "seis"     |
| Cantidad      | Porcentaje    | "50 %", "cincuenta por ciento"|
| Cantidad      | Ordinal       | "2.º", "segundo"     |
| Cantidad      | Edad           | "90 días","30 años"    |
| Cantidad      | Moneda      | "10,99 $"     |
| Cantidad      | Dimension Data     | "10 millas", "40 cm"     |
| Cantidad      | Temperatura   | "32 grados"    |
| DateTime      | N/D\*         | "6:30 p. m., 4 de febrero de 2012"      |
| DateTime      | Date          | "2 de mayo de 2017", "02/05/2017"   |
| DateTime      | Hora          | "8 a. m.", "8:00"  |
| DateTime      | DateRange     | "Del 2 de mayo al 5 de mayo"    |
| DateTime      | TimeRange     | "De las 6 p. m. a las 7 p. m."     |
| DateTime      | Duration      | "1 minuto y 45 segundos"   |
| DateTime      | Set           | "Todos los martes"     |
| URL           | N/D\*         | "https:\//www.bing.com"    |
| Email         | N/D\*         | "support@contoso.com" |
| Número de teléfono en EE. UU.  | N/D\*         | (Solo números de teléfono de EE. UU.) "(312) 555-0176" |
| Dirección IP    | N/D\*         | "10.0.0.100" |

\* En función de las entidades de entrada y extraídas, algunas entidades pueden omitir el valor `SubType`.  Todos los tipos de entidad admitidos enumerados solo están disponibles para los idiomas inglés, chino simplificado, francés, alemán y español.

### <a name="language-support"></a>Compatibilidad con idiomas

El uso de la vinculación de entidad en distintos idiomas requiere una knowledge base correspondiente en cada idioma. Para la vinculación de entidad en Text Analytics, esto significa que cada idioma compatible con el punto de conexión `entities` se vinculará al corpus de Wikipedia correspondiente en dicho idioma. Dado que el tamaño del corpus varía según el idioma, se espera que la coincidencia de la funcionalidad de vinculación de entidad también varíe. Consulte el artículo sobre la [compatibilidad con idiomas](../language-support.md#sentiment-analysis-key-phrase-extraction-and-named-entity-recognition) para más información.

## <a name="preparation"></a>Preparación

Debe tener documentos JSON en este formato: identificador, texto, idioma

Para conocer los idiomas admitidos actualmente, consulte [esta lista](../text-analytics-supported-languages.md).

El tamaño del documento debe ser inferior a 5120 caracteres por documento y puede tener hasta 1000 elementos (identificadores) por colección. La colección se envía en el cuerpo de la solicitud. El ejemplo siguiente es una ilustración del contenido que podría enviar al extremo de la vinculación de entidad.

```json
    {
        "documents": [
            {
                "id": "1",
                "language": "en",
                "text": "Jeff bought three dozen eggs because there was a 50% discount."
            },
            {
                "id": "2",
                "language": "en",
                "text": "The Great Depression began in 1929. By 1933, the GDP in America fell by 25%."
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>Paso 1: Estructurar la solicitud

Se pueden encontrar detalles sobre la definición de la solicitud en [How to call the Text Analytics API](text-analytics-how-to-call-api.md) (Cómo llamar a Text Analytics API). Recapitulamos los siguientes puntos para su comodidad:

+ Cree una solicitud **POST**. Revise la documentación de la API para esta solicitud: [API Entities](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

+ Establezca el punto de conexión de HTTP para la extracción de frases clave mediante un recurso de Text Analytics en Azure o un [contenedor de Text Analytics](text-analytics-how-to-install-containers.md) con instancias. Debe incluir `/text/analytics/v2.1/entities`. Por ejemplo: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/entities`.

+ Establezca un encabezado de solicitud para incluir la [clave de acceso](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) para las operaciones de Text Analytics.

+ En el cuerpo de la solicitud, proporcione la colección de documentos JSON que preparó para este análisis.

> [!Tip]
> Use [Postman](text-analytics-how-to-call-api.md) o abra la **consola de prueba de la API** en la [documentación](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) para estructurar y enviar una solicitud POST al servicio.

## <a name="step-2-post-the-request"></a>Paso 2: Publicar la solicitud

El análisis se realiza tras la recepción de la solicitud. Consulte la sección de [límites de datos](../overview.md#data-limits) de la introducción para obtener información sobre el tamaño y el número de solicitudes que puede enviar por minuto y segundo.

Recuerde que el servicio no tiene estado. No se almacena ningún dato en su cuenta. Los resultados se devuelven inmediatamente en la respuesta.

## <a name="step-3-view-results"></a>Paso 3: Visualización de los resultados

Todas las solicitudes POST devolverán una respuesta con formato JSON con los identificadores y las propiedades detectadas.

La salida se devuelve inmediatamente. Puede transmitir los resultados a una aplicación que acepte JSON o guardar la salida en un archivo en el sistema local y, a continuación, importarlo en una aplicación que permita ordenar, buscar y manipular los datos.

A continuación se muestra un ejemplo de la salida de la vinculación de entidad:

```json
    {
        "Documents": [
            {
                "Id": "1",
                "Entities": [
                    {
                        "Name": "Jeff",
                        "Matches": [
                            {
                                "Text": "Jeff",
                                "Offset": 0,
                                "Length": 4
                            }
                        ],
                        "Type": "Person"
                    },
                    {
                        "Name": "three dozen",
                        "Matches": [
                            {
                                "Text": "three dozen",
                                "Offset": 12,
                                "Length": 11
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Number"
                    },
                    {
                        "Name": "50",
                        "Matches": [
                            {
                                "Text": "50",
                                "Offset": 49,
                                "Length": 2
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Number"
                    },
                    {
                        "Name": "50%",
                        "Matches": [
                            {
                                "Text": "50%",
                                "Offset": 49,
                                "Length": 3
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Percentage"
                    }
                ]
            },
            {
                "Id": "2",
                "Entities": [
                    {
                        "Name": "Great Depression",
                        "Matches": [
                            {
                                "Text": "The Great Depression",
                                "Offset": 0,
                                "Length": 20
                            }
                        ],
                        "WikipediaLanguage": "en",
                        "WikipediaId": "Great Depression",
                        "WikipediaUrl": "https://en.wikipedia.org/wiki/Great_Depression",
                        "BingId": "d9364681-98ad-1a66-f869-a3f1c8ae8ef8"
                    },
                    {
                        "Name": "1929",
                        "Matches": [
                            {
                                "Text": "1929",
                                "Offset": 30,
                                "Length": 4
                            }
                        ],
                        "Type": "DateTime",
                        "SubType": "DateRange"
                    },
                    {
                        "Name": "By 1933",
                        "Matches": [
                            {
                                "Text": "By 1933",
                                "Offset": 36,
                                "Length": 7
                            }
                        ],
                        "Type": "DateTime",
                        "SubType": "DateRange"
                    },
                    {
                        "Name": "Gross domestic product",
                        "Matches": [
                            {
                                "Text": "GDP",
                                "Offset": 49,
                                "Length": 3
                            }
                        ],
                        "WikipediaLanguage": "en",
                        "WikipediaId": "Gross domestic product",
                        "WikipediaUrl": "https://en.wikipedia.org/wiki/Gross_domestic_product",
                        "BingId": "c859ed84-c0dd-e18f-394a-530cae5468a2"
                    },
                    {
                        "Name": "United States",
                        "Matches": [
                            {
                                "Text": "America",
                                "Offset": 56,
                                "Length": 7
                            }
                        ],
                        "WikipediaLanguage": "en",
                        "WikipediaId": "United States",
                        "WikipediaUrl": "https://en.wikipedia.org/wiki/United_States",
                        "BingId": "5232ed96-85b1-2edb-12c6-63e6c597a1de",
                        "Type": "Location"
                    },
                    {
                        "Name": "25",
                        "Matches": [
                            {
                                "Text": "25",
                                "Offset": 72,
                                "Length": 2
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Number"
                    },
                    {
                        "Name": "25%",
                        "Matches": [
                            {
                                "Text": "25%",
                                "Offset": 72,
                                "Length": 3
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Percentage"
                    }
                ]
            }
        ],
        "Errors": []
    }
```

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y el flujo de trabajo de vinculación de entidad mediante Text Analytics de Cognitive Services. En resumen:

+ [Entities API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) está disponible para los idiomas seleccionados.
+ Los documentos JSON del cuerpo de la solicitud incluyen un identificador, un texto y el código de idioma.
+ La solicitud POST se realiza a un punto de conexión `/entities`, con una [clave de acceso y un punto de conexión](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) personalizados, que son válidos para la suscripción.
+ La salida de respuesta, que consta de entidades vinculadas (incluidas las puntuaciones de confianza, los desplazamientos y los vínculos web de cada id. de documento) se puede usar en cualquier aplicación.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Text Analytics API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

* [Información general de Text Analytics](../overview.md)
* [Preguntas más frecuentes (P+F)](../text-analytics-resource-faq.md)</br>
* [Página del producto de Text Analytics](//go.microsoft.com/fwlink/?LinkID=759712)
