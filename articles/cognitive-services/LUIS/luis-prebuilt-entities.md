---
title: Entidades precompiladas para Language Understanding (LUIS)
titleSuffix: Azure Cognitive Services
description: LUIS incluye un conjunto de entidades precompiladas para reconocer tipos comunes de información, como fechas, horas, números, medidas y moneda. La compatibilidad de entidades precompiladas varía según la referencia cultural de la aplicación LUIS.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 10/18/2018
ms.author: diberry
ms.openlocfilehash: 2f7c724b14efd569a5993f9a9319c9004874bc43
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647602"
---
# <a name="prebuilt-entities-to-recognize-common-data-types"></a>Entidades precompiladas para reconocer tipos de datos comunes

LUIS incluye un conjunto de entidades precompiladas para reconocer tipos comunes de información, como fechas, horas, números, medidas y moneda. 

## <a name="add-a-prebuilt-entity"></a>Adición de una entidad precompilada

1. Haga clic en la aplicación para abrirla desde la página **Mis aplicaciones** y, después, haga clic en **Entidades** a la izquierda. 

1. En la página **Entities** (Entidades), haga clic en **Manage prebuilt entities** (Administrar entidades precompiladas).

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


## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Referencia de entidad precompilada](./luis-reference-prebuilt-entities.md)
