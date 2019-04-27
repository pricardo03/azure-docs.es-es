---
title: Entidades precompiladas
titleSuffix: Azure Cognitive Services
description: LUIS incluye un conjunto de entidades precompiladas para reconocer tipos comunes de información, como fechas, horas, números, medidas y moneda. La compatibilidad de entidades precompiladas varía según la referencia cultural de la aplicación LUIS.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: 0cfc4ff58cfeb65f80f9ac5ce2dd532defde5ef8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60596114"
---
# <a name="prebuilt-entities-to-recognize-common-data-types"></a>Entidades precompiladas para reconocer tipos de datos comunes

LUIS incluye un conjunto de entidades precompiladas para reconocer tipos comunes de información, como fechas, horas, números, medidas y moneda. 

## <a name="add-a-prebuilt-entity"></a>Adición de una entidad precompilada

1. Haga clic en la aplicación para abrirla desde la página **Mis aplicaciones** y, después, haga clic en **Entidades** a la izquierda. 

1. En la página **Entities** (Entidades), haga clic en **Add prebuilt entities** (Agregar entidad precompilada).

1. En el cuadro de diálogo **Add prebuilt entities** (Agregar entidades precompiladas), seleccione la entidad precompilada datetimeV2. 

    ![Cuadro de diálogo Add prebuilt entity (Agregar entidades precompiladas)](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

1. Seleccione **Listo**.

## <a name="publish-the-app"></a>Publicación de la aplicación

La manera más fácil de ver el valor de una entidad creada previamente es consultar desde el punto de conexión publicado. 

1. En la barra de herramientas superior, seleccione **Publish** (Publicar). Realice la publicación en **Production** (Producción). 

1. Cuando aparezca la notificación de éxito en verde, seleccione el vínculo **Refer to the list of endpoints** (Consultar la lista de puntos de conexión) para ver los puntos de conexión.

1. Seleccione un extremo. Se abre una nueva pestaña del explorador en ese punto de conexión. Mantenga abierta la pestaña del explorador y vaya a la sección **Test** (Prueba).

## <a name="test"></a>Prueba
Después de agregar la entidad, no es necesario entrenar la aplicación. 

Pruebe la nueva intención en el punto de conexión agregando un valor para el parámetro **q**. Use la siguiente tabla para expresiones sugeridas para **q**:

|Probar expresión|Valor de la entidad|
|--|:--|
|Reservar un vuelo mañana|19-10-2018|
|Cancelar la cita del 3 de marzo|LUIS devuelve el 3 de marzo más reciente pasado (03-03-2018) y el 3 de marzo próximo (03-03-2019) porque la expresión no especificó un año.|
|Programar una reunión a las 10:00|10:00:00|

## <a name="marking-entities-containing-a-prebuilt-entity-token"></a>Marcado de las entidades que contienen un token de entidad creada previamente
 Si tiene texto, como `HH-1234`, que desea marcar como una entidad personalizada _y_ tiene un [número creado previamente](luis-reference-prebuilt-number.md) agregado al modelo, no podrá marcar la entidad personalizada en el portal de LUIS. Se puede marcar con la API. 

 Para marcar este tipo de token, en el que parte del mismo ya está marcado con una entidad creada previamente, elimine la entidad creada previamente de la aplicación de LUIS. No es necesario entrenar la aplicación. A continuación, marque el token con su propia entidad personalizada. Después, agregue de nuevo la entidad creada previamente a la aplicación de LUIS.

 Para otro ejemplo, considere la expresión como una lista de preferencias de clase: `I want first year spanish, second year calculus, and fourth year english lit.` Si la aplicación de LUIS tiene agregado el ordinal creado previamente, `first`, `second` y `fourth` ya estarán marcados con ordinales. Si desea capturar el ordinal y la clase, puede crear una entidad compuesta y encapsularla en el ordinal creado previamente y la entidad personalizada para el nombre de clase.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Referencia de entidad precompilada](./luis-reference-prebuilt-entities.md)
