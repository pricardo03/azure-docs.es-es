---
title: Entidades precompiladas de LUIS | Microsoft Docs
description: Este artículo contiene listas de las entidades precompiladas que se incluyen en Language Understanding Intelligent Services (LUIS).
services: cognitive-services
author: cahann
manager: hsalama
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/15/2018
ms.author: cahann
ms.reviewer: v-geberr
ms.openlocfilehash: 4858de8b8517016ca385e9105ca8948b66aa683b
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321807"
---
# <a name="prebuilt-entities"></a>Entidades precompiladas

LUIS incluye un conjunto de entidades precompiladas para reconocer tipos comunes de información, como fechas, horas, números, medidas y moneda. La compatibilidad de entidades precompiladas varía según la referencia cultural de la aplicación LUIS. Para ver una lista completa de las entidades precompiladas que LUIS admite, incluso la compatibilidad por referencia cultural, consulte la [referencia de entidades precompiladas](./luis-reference-prebuilt-entities.md).

> [!NOTE]
> **builtin.datetime** está en desuso. Se sustituye por [**builtin.datetimeV2**](luis-reference-prebuilt-datetimev2.md), que permite reconocer los intervalos de fechas y horas, así como mejorar el reconocimiento de las fechas y horas ambiguas.

## <a name="add-a-prebuilt-entity"></a>Adición de una entidad precompilada

1. Haga clic en la aplicación para abrirla desde la página **Mis aplicaciones** y, después, haga clic en **Entidades** a la izquierda. 
2. En la página **Entities** (Entidades), haga clic en **Manage prebuilt entities** (Administrar entidades precompiladas).

    ![Página Entities (Entidades): Manage prebuilt entities (Administrar entidades precompiladas)](./media/luis-use-prebuilt-entity/add-prebuilt-entity-button.png)
3. En el cuadro de diálogo **Add prebuilt entities** (Agregar entidades precompiladas), haga clic en la entidad precompilada que desea agregar (por ejemplo, "datetimeV2"). A continuación, haga clic en **Guardar**.

    ![Cuadro de diálogo Add prebuilt entity (Agregar entidades precompiladas)](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

## <a name="use-a-prebuilt-number-entity"></a>Uso de una entidad numérica precompilada
Cuando una entidad precompilada se incluye en la aplicación, sus predicciones se incluyen en la aplicación publicada. El comportamiento de las entidades precompiladas ya está entrenado y **no** se puede modificar. Siga estos pasos para ver cómo funciona una entidad precompilada:

1. Agregue una entidad de **número** a la aplicación, y después [entrene](interactive-test.md) y [publique](PublishApp.md) la aplicación.
2. Haga clic en la dirección URL del punto de conexión en la página **Publicar aplicación** para abrir el punto de conexión de LUIS en un explorador web. 
3. Agregue una expresión a la dirección URL que contenga una expresión numérica. Por ejemplo, puede escribir `buy two plane ticktets` y ver que LUIS identifica `two` como una entidad `builtin.number`, e identifica `2` como su valor en el campo `resolution`. El campo `resolution` le ayuda a resolver números y fechas en forma canónica que es más fácil de usar para la aplicación cliente. 

    ![expresión en el explorador que contiene una entidad numérica](./media/luis-use-prebuilt-entity/browser-query.png)

LUIS puede reconocer inteligentemente números que no están en forma no estándar. Pruebe diferentes expresiones numéricas en sus expresiones y vea lo que LUIS devuelve.

En el siguiente ejemplo, se muestra una respuesta JSON de LUIS, que incluye la resolución del valor 24, para la expresión "dos docenas".

```json
{
  "query": "order two dozen tickets for group travel",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.905443209
  },
  "entities": [
    {
      "entity": "two dozen",
      "type": "builtin.number",
      "startIndex": 6,
      "endIndex": 14,
      "resolution": {
        "value": "24"
      }
    }
  ]
}
```
## <a name="use-a-prebuilt-datetimev2-entity"></a>Uso de una entidad precompilada datetimeV2
La entidad **datetimeV2** precompilada reconoce fechas, horas, rangos de fechas y duraciones de tiempo. Siga estos pasos para ver cómo funciona la entidad precompilada `datetimeV2`:

1. Agregue una entidad **datetimeV2** a la aplicación, y después [entrene](interactive-test.md) y [publique](PublishApp.md) la aplicación.
2. Haga clic en la dirección URL del punto de conexión en la página **Publicar aplicación** para abrir el punto de conexión de LUIS en un explorador web. 
3. Agregue una expresión a la dirección URL que contenga un rango de fechas. Por ejemplo, puede escribir `book a flight tomorrow` y ver que LUIS identifica `tomorrow` como una entidad `builtin.datetimeV2.date`, e identifica la fecha del día siguiente como su valor en el campo `resolution`. 

El siguiente ejemplo muestra cómo sería la respuesta de JSON de LUIS si la fecha de hoy fuera el 31 de octubre de 2017.

```json
{
  "query": "book a flight tomorrow",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.9063408
  },
  "entities": [
    {
      "entity": "tomorrow",
      "type": "builtin.datetimeV2.date",
      "startIndex": 14,
      "endIndex": 21,
      "resolution": {
        "values": [
          {
            "timex": "2017-11-01",
            "type": "date",
            "value": "2017-11-01"
          }
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Referencia de entidad precompilada](./luis-reference-prebuilt-entities.md)
