---
title: 'Aptitud de búsqueda cognitiva Opinión: Azure Search'
description: Extraiga una puntuación de opinión positiva-negativa del texto en una canalización de enriquecimiento de Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 4b8913d64a3df8799ba1d73972121ef331aaac81
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53314081"
---
#   <a name="sentiment-cognitive-skill"></a>Aptitud cognitiva Opinión

La aptitud **Opinión** evalúa el texto no estructurado a lo largo de una continuidad positiva-negativa y, para cada registro, devuelve un valor numérico entre 0 y 1. Las puntuaciones próximas a 1 indican una opinión positiva y las puntuaciones próximas a 0 indican una opinión negativa.

> [!NOTE]
> A partir del 21 de diciembre de 2018, podrá asociar un recurso de Cognitive Services con un conjunto de aptitudes de Azure Search. Esto nos permitirá empezar a cobrar por la ejecución del conjunto de aptitudes. En esta fecha, también empezaremos a cobrar por la extracción de imágenes como parte de la fase de descifrado de documentos. La extracción de texto de documentos continuará ofreciéndose sin costo adicional.
>
> La ejecución de aptitudes integradas se cobrará a los [precios de pago por uso existentes de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/). La extracción de imágenes se cobrará al precio de la versión preliminar, tal y como se describe en la [página de precios de Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400). [Más información](cognitive-search-attach-cognitive-services.md).

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SentimentSkill

## <a name="data-limits"></a>Límites de datos
El tamaño máximo de un registro debe tener 5000 caracteres, medido por `String.Length`. Si tiene que dividir los datos antes de enviarlos al analizador de opiniones, use la [aptitud División de texto](cognitive-search-skill-textsplit.md).


## <a name="skill-parameters"></a>Parámetros de la aptitud

Los parámetros distinguen mayúsculas de minúsculas.

| Nombre de parámetro |                      |
|----------------|----------------------|
| defaultLanguageCode | (Opcional) Es el código de idioma que se aplicará a los documentos que no especifiquen el lenguaje de forma explícita. <br/> Vea [Full list of supported languages](../cognitive-services/text-analytics/text-analytics-supported-languages.md) (Lista completa de idiomas admitidos). |

## <a name="skill-inputs"></a>Entradas de la aptitud 

| Nombre de entrada | DESCRIPCIÓN |
|--------------------|-------------|
| text | Texto que se va a analizar.|
| languageCode  |  (Opcional) Cadena que indica el idioma de los registros. Si esta no se especifica este parámetro, se usa el valor predeterminado "en". <br/>Vea [Full list of supported languages](../cognitive-services/text-analytics/text-analytics-supported-languages.md) (Lista completa de idiomas admitidos).|

## <a name="skill-outputs"></a>Salidas de la aptitud

| Nombre de salida | DESCRIPCIÓN |
|--------------------|-------------|
| de la aplicación | Un valor entre 0 y 1 que representa la opinión del texto analizado. Valores próximos a 0 tienen opiniones negativo, los valores próximos a 0,5 tienen opiniones neutras y los valores próximos a 1 tienen opiniones positivas.|


##  <a name="sample-definition"></a>Definición de ejemplo

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/languagecode"
        }
    ],
    "outputs": [
        {
            "name": "score",
            "targetName": "mySentiment"
        }
    ]
}
```

##  <a name="sample-input"></a>Entrada de ejemplo

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "I had a terrible time at the hotel. The staff was rude and the food was awful.",
                "languageCode": "en"
            }
        }
    ]
}
```


##  <a name="sample-output"></a>Salida de ejemplo

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "score": 0.01
            }
        }
    ]
}
```

## <a name="notes"></a>Notas
Si no hay nada, no se devuelve una puntuación de la opinión para dichos registros.

## <a name="error-cases"></a>Casos de error
Si no se admite un idioma, se genera un error y no se devuelve ninguna puntuación de opinión.

## <a name="see-also"></a>Otras referencias

+ [Aptitudes predefinidas](cognitive-search-predefined-skills.md)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)