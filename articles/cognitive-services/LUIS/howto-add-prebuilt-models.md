---
title: Modelos precompilados para Language Understanding
titleSuffix: Azure Cognitive Services
description: LUIS incluye un conjunto de modelos precompilados para agregar rápidamente escenarios de usuario comunes y de conversación.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 2e90a5f3f7cd2cfde2a2ead26674d2348a24ae6f
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506787"
---
# <a name="add-prebuilt-models-for-common-usage-scenarios"></a>Adición de modelos precompilados en escenarios de uso común 

LUIS incluye un conjunto de intenciones precompiladas de los dominios creados previamente para agregar rápidamente intenciones y expresiones comunes. Esta es una forma rápida y fácil de agregar habilidades a la aplicación cliente de conversación sin tener que diseñar los modelos correspondientes a esas habilidades. 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

## <a name="add-a-prebuilt-domain"></a>Incorporación de un dominio creado previamente

1. En la página **My Apps** (Mis aplicaciones), seleccione su aplicación. Esta opción abre su aplicación a la sección **Build** (Compilar) de la aplicación. 

1. En la página **Intents** (Intenciones), seleccione **Add prebuilt domains** (Agregar dominios creados previamente) de la barra de herramientas inferior izquierda. 

1. Seleccione la intención **Calendar** (Calendario) y, a continuación, seleccione **Add domain** (Agregar dominio).

    ![Agregar un dominio de Calendario creado previamente](./media/luis-prebuilt-domains/add-prebuilt-domain.png)

1. Seleccione **Intents** (Intenciones)en el panel de navegación izquierdo para ver las intenciones de calendario. Cada intención de este dominio tiene el prefijo `Calendar.`. Junto con las expresiones, se agregan dos entidades de este dominio a la aplicación: `Calendar.Location` y `Calendar.Subject`. 

### <a name="train-and-publish"></a>Entrenar y publicar

1. Una vez haya agregado el dominio, seleccione **Train** (Entrenar) en la barra de herramientas superior derecha para entrenar la aplicación. 

1. En la barra de herramientas superior, seleccione **Publish** (Publicar). Realice la publicación en **Production** (Producción). 

1. Cuando aparezca la notificación de éxito en verde, seleccione el vínculo **Refer to the list of endpoints** (Consultar la lista de puntos de conexión) para ver los puntos de conexión.

1. Seleccione un extremo. Se abre una nueva pestaña del explorador en ese punto de conexión. Mantenga abierta la pestaña del explorador y vaya a la sección **Test** (Prueba).

### <a name="test"></a>Prueba

Pruebe la nueva intención en el punto de conexión agregando un valor para el parámetro **q**: `Schedule a meeting with John Smith in Seattle next week`.

LUIS devuelve la intención correcta y el asunto de la reunión:

```json
{
  "query": "Schedule a meeting with John Smith in Seattle next week",
  "topScoringIntent": {
    "intent": "Calendar.Add",
    "score": 0.824783146
  },
  "entities": [
    {
      "entity": "a meeting with john smith",
      "type": "Calendar.Subject",
      "startIndex": 9,
      "endIndex": 33,
      "score": 0.484055847
    }
  ]
}
```

## <a name="add-a-prebuilt-intent"></a>Agregar una intención creada previamente

1. En la página **My Apps** (Mis aplicaciones), seleccione su aplicación. Esta opción abre su aplicación a la sección **Build** (Compilar) de la aplicación. 

1. En la página **Intents** (Intenciones), seleccione **Add prebuilt intent** (Agregar intención creada previamente) en la barra de herramientas que se encuentra sobre la lista de intenciones. 

1. Seleccione la intención **Utilities.Cancel** en el cuadro de diálogo emergente. 

    ![Agregar una intención creada previamente](./media/luis-prebuilt-intents/prebuilt-intents-ddl.png)

1. Haga clic en el botón **Done** (Listo).

### <a name="train-and-test"></a>Entrenar y probar

1. Una vez haya agregado la intención, seleccione **Train** (Entrenar) en la barra de herramientas superior derecha para entrenar la aplicación. 

1. Pruebe la nueva intención seleccionando **Test** (Probar) en la barra de herramientas derecha. 

1. En el cuadro de texto, escriba expresiones para cancelar lo siguiente:

    |Probar expresión|Puntuación de predicción|
    |--|:--|
    |Quiero cancelar el vuelo.|0,67|
    |Cancelar la compra.|0,52|
    |Cancelar la reunión.|0,56|

    ![Probar una intención creada previamente](./media/luis-prebuilt-intents/test.png)

## <a name="add-a-prebuilt-entity"></a>Adición de una entidad precompilada

1. Haga clic en la aplicación para abrirla desde la página **Mis aplicaciones** y, después, haga clic en **Entidades** a la izquierda. 

1. En la página **Entities** (Entidades), haga clic en **Add prebuilt entities** (Agregar entidad precompilada).

1. En el cuadro de diálogo **Add prebuilt entities** (Agregar entidades precompiladas), seleccione la entidad precompilada datetimeV2. 

    ![Cuadro de diálogo Add prebuilt entity (Agregar entidades precompiladas)](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

1. Seleccione **Listo**. Después de agregar la entidad, no es necesario entrenar la aplicación. 

## <a name="publish-to-view-prebuilt-model-from-prediction-endpoint"></a>Publicación para visualizar el modelo precompilado desde el punto de conexión de predicción

La manera más fácil de ver el valor de un modelo precompilado es consultar desde el punto de conexión publicado. 

## <a name="marking-entities-containing-a-prebuilt-entity-token"></a>Marcado de las entidades que contienen un token de entidad creada previamente
 Si tiene texto, como `HH-1234`, que desea marcar como una entidad personalizada _y_ tiene un [número creado previamente](luis-reference-prebuilt-number.md) agregado al modelo, no podrá marcar la entidad personalizada en el portal de LUIS. Se puede marcar con la API. 

 Para marcar este tipo de token, en el que parte del mismo ya está marcado con una entidad creada previamente, elimine la entidad creada previamente de la aplicación de LUIS. No es necesario entrenar la aplicación. A continuación, marque el token con su propia entidad personalizada. Después, agregue de nuevo la entidad creada previamente a la aplicación de LUIS.

 Para otro ejemplo, considere la expresión como una lista de preferencias de clase: `I want first year spanish, second year calculus, and fourth year english lit.` Si la aplicación de LUIS tiene agregado el ordinal creado previamente, `first`, `second` y `fourth` ya estarán marcados con ordinales. Si desea capturar el ordinal y la clase, puede crear una entidad compuesta y encapsularla en el ordinal creado previamente y la entidad personalizada para el nombre de clase.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Compilación de un modelo a partir de archivo. csv con las API REST](./luis-tutorial-node-import-utterances-csv.md)
