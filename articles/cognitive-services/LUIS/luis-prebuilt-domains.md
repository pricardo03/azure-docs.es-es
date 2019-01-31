---
title: Dominios creados previamente para Language Understanding (LUIS)
titleSuffix: Azure Cognitive Services
description: LUIS incluye un conjunto de dominios creados previamente para agregar rápidamente escenarios de usuario comunes y de conversación.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: cgronlun
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/24/2019
ms.author: diberry
ms.openlocfilehash: 74b28c926e077bb35122d3435fcfd93816acc9fd
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55215221"
---
# <a name="add-prebuilt-domains-for-common-usage-scenarios"></a>Agregar dominios creados previamente en escenarios de uso común 

LUIS incluye un conjunto de intenciones precompiladas de los dominios creados previamente para agregar rápidamente intenciones y expresiones comunes. Esta es una forma rápida y fácil de agregar habilidades a la aplicación cliente de conversación sin tener que diseñar los modelos correspondientes a esas habilidades. 

## <a name="add-a-prebuilt-domain"></a>Incorporación de un dominio creado previamente

1. En la página **My Apps** (Mis aplicaciones), seleccione su aplicación. Esta opción abre su aplicación a la sección **Build** (Compilar) de la aplicación. 

1. En la página **Intents** (Intenciones), seleccione **Add prebuilt domains** (Agregar dominios creados previamente) de la barra de herramientas inferior izquierda. 

1. Seleccione la intención **Calendar** (Calendario) y, a continuación, seleccione **Add domain** (Agregar dominio).

    ![Agregar un dominio de Calendario creado previamente](./media/luis-prebuilt-domains/add-prebuilt-domain.png)

1. Seleccione **Intents** (Intenciones)en el panel de navegación izquierdo para ver las intenciones de calendario. Cada intención de este dominio tiene el prefijo `Calendar.`. Junto con las expresiones, se agregan dos entidades de este dominio a la aplicación: `Calendar.Location` y `Calendar.Subject`. 

## <a name="train-and-publish"></a>Entrenar y publicar

1. Una vez haya agregado el dominio, seleccione **Train** (Entrenar) en la barra de herramientas superior derecha para entrenar la aplicación. 

1. En la barra de herramientas superior, seleccione **Publish** (Publicar). Realice la publicación en **Production** (Producción). 

1. Cuando aparezca la notificación de éxito en verde, seleccione el vínculo **Refer to the list of endpoints** (Consultar la lista de puntos de conexión) para ver los puntos de conexión.

1. Seleccione un extremo. Se abre una nueva pestaña del explorador en ese punto de conexión. Mantenga abierta la pestaña del explorador y vaya a la sección **Test** (Prueba).

## <a name="test"></a>Prueba

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

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Referencia del dominio creado previamente](./luis-reference-prebuilt-domains.md)
