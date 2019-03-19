---
title: Uso del reconocimiento de entidades con Text Analytics API
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo reconocer entidades con la API REST de Text Analytics.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: 1ea34d69c867d2d14496320f497df2ece5280e0c
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58009345"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics-preview"></a>Cómo usar el reconocimiento de entidades con nombre en Text Analytics (versión preliminar)

[Entity Recognition API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) toma el texto no estructurado y, para cada documento JSON, devuelve una lista de entidades desambiguadas con vínculos a más información en la web (Wikipedia y Bing). 

## <a name="entity-linking-and-named-entity-recognition"></a>Vinculación de entidad y reconocimiento de entidades con nombre

Text Analytics `entities` admite el punto de conexión tanto denominado reconocimiento de entidades (ER) y la vinculación de entidad.

### <a name="entity-linking"></a>Entity Linking
La vinculación de entidad es la capacidad de identificar y desambiguar la identidad de una entidad que se encuentra en el texto (por ejemplo, determinar si se está usando "Marte" como el planeta o como el dios romano de la guerra). Este proceso requiere la presencia de una knowledge base a la que se vinculan las entidades reconocidas (Wikipedia se usa como knowledge base del punto de conexión `entities` de Text Analytics).

En Text Analytics [versión 2.0](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634), solo está disponible la vinculación de entidad.

### <a name="named-entity-recognition-ner"></a>Reconocimiento de entidades con nombre (NER)
El reconocimiento de entidades con nombre (NER) es la capacidad de identificar diferentes entidades en el texto y clasificarlas en clases predefinidas. Las clases de entidades admitidas se enumeran a continuación.

En Text Analytics [versión preliminar 2.1](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634), están disponibles la vinculación de entidad y el reconocimiento de entidades con nombre (NER).

### <a name="language-support"></a>Compatibilidad con idiomas

El uso de la vinculación de entidad en distintos idiomas requiere una knowledge base correspondiente en cada idioma. Para la vinculación de entidad en Text Analytics, esto significa que cada idioma compatible con el punto de conexión `entities` se vinculará al corpus de Wikipedia correspondiente en dicho idioma. Dado que el tamaño del corpus varía según el idioma, se espera que la coincidencia de la funcionalidad de vinculación de entidad también varíe.

## <a name="supported-types-for-named-entity-recognition"></a>Tipos admitidos para el reconocimiento de entidades con nombre

| Type  | SubType | Ejemplo |
|:-----------   |:------------- |:---------|
| Persona        | N/D\*         | "Jeff", "Bill Gates"     |
| Ubicación      | N/D\*         | "Redmond, Washington", "París"  |
| Organización  | N/D\*         | "Microsoft"   |
| Cantidad      | Number        | "6", "seis"     | 
| Cantidad      | Porcentaje    | "50 %", "cincuenta por ciento"| 
| Cantidad      | Ordinal       | "2.º", "segundo"     | 
| Cantidad      | NumberRange   | "De 4 a 8"     | 
| Cantidad      | Edad           | "90 días","30 años"    | 
| Cantidad      | Moneda      | "10,99 $"     | 
| Cantidad      | Dimension Data     | "10 millas", "40 cm"     | 
| Cantidad      | Temperatura   | "32 grados"    |
| DateTime      | N/D\*         | "6:30 p. m., 4 de febrero de 2012"      | 
| DateTime      | Date          | "2 de mayo de 2017", "02/05/2017"   | 
| Date Time     | Hora          | "8 a. m.", "8:00"  | 
| DateTime      | DateRange     | "Del 2 de mayo al 5 de mayo"    | 
| DateTime      | TimeRange     | "De las 6 p. m. a las 7 p. m."     | 
| DateTime      | Duration      | "1 minuto y 45 segundos"   | 
| DateTime      | Set           | "Todos los martes"     | 
| DateTime      | TimeZone      |    | 
| URL           | N/D\*         | "<https://www.bing.com>"    |
| Email         | N/D\*         | "support@contoso.com" |

\* En función de las entidades de entrada y extraídas, algunas entidades pueden omitir el valor `SubType`.



## <a name="preparation"></a>Preparación

Debe tener documentos JSON en este formato: identificador, texto, idioma

Para conocer los idiomas admitidos actualmente, consulte [esta lista](../text-analytics-supported-languages.md).

El tamaño del documento debe ser inferior a 5120 caracteres por documento y puede tener hasta 1000 elementos (identificadores) por colección. La colección se envía en el cuerpo de la solicitud. El ejemplo siguiente es una ilustración del contenido que podría enviar al extremo de la vinculación de entidad.

```
{"documents": [{"id": "1",
                "language": "en",
                "text": "Jeff bought three dozen eggs because there was a 50% discount."
                },
               {"id": "2",
                "language": "en",
                "text": "The Great Depression began in 1929. By 1933, the GDP in America fell by 25%."
                }
               ]
}
```    
    
## <a name="step-1-structure-the-request"></a>Paso 1: Estructurar la solicitud

Se pueden encontrar detalles sobre la definición de la solicitud en [How to call the Text Analytics API](text-analytics-how-to-call-api.md) (Cómo llamar a Text Analytics API). Recapitulamos los siguientes puntos para su comodidad:

+ Cree una solicitud **POST**. Revise la documentación de la API para esta solicitud: [Entity Linking API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634)

+ Establezca el punto de conexión HTTP para la extracción de la entidad. Debe incluir el recurso `/entities`: `https://[your-region].api.cognitive.microsoft.com/text/analytics/v2.1-preview/entities`

+ Establezca un encabezado de solicitud para incluir la clave de acceso para las operaciones de Text Analytics. Para obtener más información, consulte [How to find endpoints and access keys](text-analytics-how-to-access-key.md) (Cómo buscar puntos de conexión y claves de acceso).

+ En el cuerpo de la solicitud, proporcione la colección de documentos JSON que preparó para este análisis.

> [!Tip]
> Use [Postman](text-analytics-how-to-call-api.md) o abra la **consola de prueba de la API** en la [documentación](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) para estructurar y enviar una solicitud POST al servicio.

## <a name="step-2-post-the-request"></a>Paso 2: Publicar la solicitud

El análisis se realiza tras la recepción de la solicitud. El servicio acepta hasta 100 solicitudes por minuto. Cada solicitud puede tener un máximo de 1 MB.

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

+ [Entities API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) está disponible para los idiomas seleccionados.
+ Los documentos JSON del cuerpo de la solicitud incluyen un identificador, un texto y el código de idioma.
+ La solicitud POST se realiza a un punto de conexión `/entities`, con una [clave de acceso y un punto de conexión](text-analytics-how-to-access-key.md) personalizados, que son válidos para la suscripción.
+ La salida de respuesta, que consta de entidades vinculadas (incluidas las puntuaciones de confianza, los desplazamientos y los vínculos web de cada id. de documento) se puede usar en cualquier aplicación.

## <a name="see-also"></a>Vea también 

 [Información general de Text Analytics](../overview.md)  
 [Preguntas más frecuentes (P+F)](../text-analytics-resource-faq.md)</br>
 [Página del producto de Text Analytics](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Text Analytics API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634)
