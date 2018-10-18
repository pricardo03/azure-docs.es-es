---
title: Aptitud de Cognitive Search para la detección de idiomas (Azure Search) | Microsoft Docs
description: Evalúa el texto no estructurado y, en cada registro, devuelve un identificador de idioma con una puntuación que indica la solidez del análisis en una canalización de enriquecimiento de Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 2fd1c1ec0d2442afd6367e1d35af6f798dced2c7
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733285"
---
#   <a name="language-detection-cognitive-skill"></a>Aptitud cognitiva para la detección de idiomas

La aptitud para la**detección de idiomas** no solo ofrece la posibilidad de usar hasta 120 idiomas, además, detecta el idioma del texto de entrada y usa un código de idioma único para informar acerca de cada documento enviado en la solicitud. El código de idioma se empareja con una puntuación que indica la intensidad del análisis.

Esta funcionalidad es especialmente útil cuando necesita proporcionar el idioma del texto como entrada para otras aptitudes (por ejemplo, la [aptitud de análisis de opiniones](cognitive-search-skill-sentiment.md) o la [aptitud de división de texto](cognitive-search-skill-textsplit.md)).

> [!NOTE]
> Cognitive Search está disponible en la versión preliminar pública. La ejecución del conjunto de habilidades y la extracción y normalización de imágenes se ofrecen actualmente de forma gratuita. Más adelante, se anunciarán los precios de estas funcionalidades. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.LanguageDetectionSkill

## <a name="data-limits"></a>Límites de datos
El tamaño máximo de un registro debe tener 50 000 caracteres según lo que mida `String.Length`. Si tiene que dividir los datos antes de enviarlos al analizador de opiniones, puede usar la [aptitud de división de texto](cognitive-search-skill-textsplit.md).

## <a name="skill-inputs"></a>Entradas de aptitudes

Los parámetros distinguen mayúsculas de minúsculas.

| Entradas     | DESCRIPCIÓN |
|--------------------|-------------|
| text | Texto que se va a analizar.|

## <a name="skill-outputs"></a>Salidas de aptitudes

| Nombre de salida    | DESCRIPCIÓN |
|--------------------|-------------|
| languageCode | El código de idioma ISO 6391 para el idioma identificado. Por ejemplo, "en". |
| languageName | El nombre del idioma. Por ejemplo, "inglés". |
| de la aplicación | Un valor entre 0 y 1. La probabilidad de que el lenguaje esté correctamente identificado. La puntuación puede ser inferior a 1 si la oración tiene distintos idiomas.  |

##  <a name="sample-definition"></a>Definición de ejemplo

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill ",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      }
    ],
    "outputs": [
      {
        "name": "languageCode",
        "targetName": "myLanguageCode"
      },
      {
        "name": "languageName",
        "targetName": "myLanguageName"
      },
      {
        "name": "score",
        "targetName": "myLanguageScore"
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
        "data":
           {
             "text": "Glaciers are huge rivers of ice that ooze their way over land, powered by gravity and their own sheer weight. "
           }
      },
      {
        "recordId": "2",
        "data":
           {
             "text": "Estamos muy felices de estar con ustedes."
           }
      }
    ]
```


##  <a name="sample-output"></a>Salida de ejemplo

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
            {
              "languageCode": "en",
              "languageName": "English",
              "score": 1,
            }
      },
      {
        "recordId": "2",
        "data":
            {
              "languageCode": "es",
              "languageName": "Spanish",
              "score": 1,
            }
      }
    ]
}
```


## <a name="error-cases"></a>Casos de error
Si el texto está escrito en un idioma no compatible, se genera un error y no se devuelve ningún identificador de idioma.

## <a name="see-also"></a>Otras referencias

+ [Aptitudes predefinidas](cognitive-search-predefined-skills.md)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)
