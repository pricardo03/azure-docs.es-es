---
title: Intenciones creadas previamente para Language Understanding
titleSuffix: Azure Cognitive Services
description: LUIS incluye un conjunto de intenciones creadas previamente para agregar rápidamente escenarios de usuario comunes y de conversación.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: eb9d9b366dd9c4dbb02e9c79a908ba3b1bf1e3f0
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68637879"
---
# <a name="add-prebuilt-intents-for-common-intents"></a>Agregar intenciones creadas previamente a las intenciones comunes 

LUIS incluye un conjunto de intenciones precompiladas de los dominios creados previamente para agregar rápidamente intenciones y expresiones comunes. Esta es una forma rápida y fácil de agregar habilidades a la aplicación cliente de conversación sin tener que diseñar los modelos correspondientes a esas habilidades. 

## <a name="add-a-prebuilt-intent"></a>Agregar una intención creada previamente

1. En la página **My Apps** (Mis aplicaciones), seleccione su aplicación. Esta opción abre su aplicación a la sección **Build** (Compilar) de la aplicación. 

1. En la página **Intents** (Intenciones), seleccione **Add prebuilt intent** (Agregar intención creada previamente) en la barra de herramientas que se encuentra sobre la lista de intenciones. 

1. Seleccione la intención **Utilities.Cancel** en el cuadro de diálogo emergente. 

    ![Agregar una intención creada previamente](./media/luis-prebuilt-intents/prebuilt-intents-ddl.png)

1. Haga clic en el botón **Done** (Listo).

## <a name="train-and-test"></a>Entrenar y probar

1. Una vez haya agregado la intención, seleccione **Train** (Entrenar) en la barra de herramientas superior derecha para entrenar la aplicación. 

1. Pruebe la nueva intención seleccionando **Test** (Probar) en la barra de herramientas derecha. 

1. En el cuadro de texto, escriba expresiones para cancelar lo siguiente:

    |Probar expresión|Puntuación de predicción|
    |--|:--|
    |Quiero cancelar el vuelo.|0,67|
    |Cancelar la compra.|0,52|
    |Cancelar la reunión.|0,56|

    ![Probar una intención creada previamente](./media/luis-prebuilt-intents/test.png)

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Entidades precompiladas](./luis-prebuilt-entities.md)
