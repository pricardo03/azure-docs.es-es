---
title: Creación de una nueva aplicación
titleSuffix: Language Understanding - Azure Cognitive Services
description: Cree y administre las aplicaciones en la página web de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 8275965e84021c41a3d0b3d13a4fb71d22090757
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53139918"
---
# <a name="create-a-new-luis-app-in-the-luis-portal"></a>Creación de una aplicación de LUIS en el portal de LUIS
Hay un par de formas de crear aplicaciones de LUIS. Puede crear una aplicación de LUIS en el portal de [LUIS](https://www.luis.ai), o bien mediante las [API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) de creación de LUIS.

## <a name="using-the-luis-portal"></a>Mediante el portal de LUIS
Puede crear una aplicación en el portal de LUIS de varias maneras:

* Comience con una aplicación vacía y cree intenciones, expresiones y entidades.
* Comience con una aplicación vacía y agregue un [dominio creado previamente](luis-how-to-use-prebuilt-domains.md).
* Importe una aplicación de LUIS desde un archivo JSON que ya contenga intenciones, expresiones y entidades.

## <a name="using-the-authoring-apis"></a>Mediante las API de creación
Puede crear una aplicación con las API de creación de dos maneras:

* [Comience](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) con una aplicación vacía y cree intenciones, expresiones y entidades.
* [Empiece](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/59104e515aca2f0b48c76be5) con un dominio creado previamente.  


<a name="export-app"></a>
<a name="import-new-app"></a>
<a name="delete-app"></a>
 

## <a name="create-new-app-in-luis"></a>Creación de una aplicación en LUIS

1. En la página **My Apps** (Mis aplicaciones), haga clic en **Create new app** (Crear aplicación).

    ![Lista de aplicaciones de LUIS](./media/luis-create-new-app/apps-list.png)


2. En el cuadro de diálogo asigne el nombre "TravelAgent" a la aplicación.

    ![Cuadro de diálogo para crear una aplicación](./media/luis-create-new-app/create-app.png)

3. Elija la referencia cultural de la aplicación (para la aplicación TravelAgent, seleccione Inglés) y, después, haga clic en **Done** (Listo). 

    > [!NOTE]
    > La referencia cultural no se puede cambiar una vez creada la aplicación. 

    

<!--

## Import new app
You can set the name (50 char max), version (10 char max), and description of an app in the JSON file. Examples of application JSON files are available at [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/Examples-BookFlight).

1. On **My Apps** page, select **Import new app**.
2. In the **Import new app** dialog, select the JSON file defining the LUIS app.

    ![Import a new app dialog](./media/luis-create-new-app/import-app.png)

## Export app
1. On **My Apps** page, select the ellipsis (***...***) at the end of the app row.

    [![Screenshot of pop-up dialog of per-app actions](media/luis-create-new-app/apps-list.png "Screenshot of pop-up dialog of per-app actions")](media/luis-create-new-app/three-dots.png#lightbox)

2. Select **Export app** from the menu. 

## Rename app

1. On **My Apps** page, select the ellipsis (***...***) at the end of the app row. 
2. Select **Rename** from the menu.
3. Enter the new name of the app and select **Done**.

## Delete app

> [!CAUTION]
> You are deleting the app for all collaborators and the owner. [Export](#export-app) the app before deleting it. 

1. On **My Apps** page, select the ellipsis (***...***) at the end of the app row. 
2. Select **Delete** from the menu.
3. Select **Ok** in the confirmation window.

## Export endpoint logs
The logs contain the Query, UTC time, and LUIS JSON response.

1. On **My Apps** page, select the ellipsis (***...***) at the end of the app row. 
2. Select **Export endpoint logs** from the menu.

```
Query,UTC DateTime,Response
text i'm driving and will be 30 minutes late to the meeting,02/13/2018 15:18:43,"{""query"":""text I'm driving and will be 30 minutes late to the meeting"",""intents"":[{""intent"":""None"",""score"":0.111048922},{""intent"":""SendMessage"",""score"":0.987501}],""entities"":[{""entity"":""i ' m driving and will be 30 minutes late to the meeting"",""type"":""Message"",""startIndex"":5,""endIndex"":58,""score"":0.162995353}]}"
```
-->
## <a name="next-steps"></a>Pasos siguientes

La primera tarea en la aplicación es [agregar intenciones](luis-how-to-add-intents.md).