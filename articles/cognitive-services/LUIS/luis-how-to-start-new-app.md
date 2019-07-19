---
title: Creación de una nueva aplicación
titleSuffix: Language Understanding - Azure Cognitive Services
description: Cree y administre las aplicaciones en la página web de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: b8b0cebf4ba47f875caacfcfbf89b84551b41333
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341852"
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

## <a name="import-an-app-from-file"></a>Importación de una aplicación desde un archivo

1. En la página **My Apps** (Mis aplicaciones), haga clic en **Import new app** (Importar aplicación nueva).
1. En el cuadro de diálogo emergente, seleccione un archivo JSON de aplicación válido y, luego, seleccione **Done** (Listo).

### <a name="import-errors"></a>Errores de importación

Errores posibles son: 

* Una aplicación con ese nombre ya existe. Para solucionar esto, vuelva a importar la aplicación y establezca el **nombre opcional** en un nuevo nombre. 

## <a name="export-app-for-backup"></a>Exportación de una aplicación para copia de seguridad

1. En la página **My Apps** (Mis aplicaciones), seleccione **Exportar**.
1. Seleccione **Export as JSON** (Exportar como JSON). El explorador descarga la versión activa de la aplicación.
1. Agregue este archivo al sistema de copia de seguridad para archivar el modelo.

## <a name="export-app-for-containers"></a>Exportación de una aplicación para contenedores

1. En la página **My Apps** (Mis aplicaciones), seleccione **Exportar**.
1. Seleccione **Export as container** (Exportar como contenedor) y, luego, seleccione qué ranura publicada (producción o ensayo) quiere exportar.
1. Use este archivo con su [contenedor de LUIS](luis-container-howto.md). 

    Si está interesado en exportar un modelo entrenado, pero que aún no se ha publicado, para usarlo con el contenedor de LUIS, vaya a la página **Versions** (Versiones) y expórtelo desde allí. 

## <a name="delete-app"></a>Eliminar la aplicación

1. En la página **My Apps** (Mis aplicaciones), haga clic en los tres puntos (...) situados al final de la fila de la aplicación.
1. Seleccione **Delete** (Eliminar) en el menú.
1. En la ventana de confirmación, haga clic en **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes

La primera tarea en la aplicación es [agregar intenciones](luis-how-to-add-intents.md).
