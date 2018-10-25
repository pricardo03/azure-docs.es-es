---
title: 'Creación de una base de conocimiento: QnA Maker'
titleSuffix: Azure Cognitive Services
description: La incorporación de charlas al bot lo hace más atractivo y conversacional. QnA Maker permite agregar fácilmente un conjunto previamente rellenado de las principales charlas a la KB. Esto puede ser un punto de partida para las charlas del bot y permite ahorrar el tiempo y el costo que supone escribirlas desde cero.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: c9d36014bc364d8b016221e6b9ff380b0bd4b8ed
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "48854174"
---
# <a name="create-a-knowledge-base"></a>Creación de una base de conocimientos

QnA Maker facilita la incorporación de orígenes de datos existentes a la hora de crear una base de conocimiento. Puede crear una nueva base de conocimiento de QnA Maker a partir de los siguientes tipos de documentos:

<!-- added for scanability -->
* Páginas de preguntas más frecuentes
* Manuales de productos
* Documentos estructurados

## <a name="steps"></a>Pasos

1. Inicie sesión en el [portal de QnA Maker](https://qnamaker.ai) con las credenciales de Azure y seleccione **Create a knowledge base** (Crear una base de conocimientos).

2. Si aún no ha creado un servicio QnA Maker, haga clic en **Create a QnA service** (Crear un servicio QnA). 

3. Seleccione el inquilino de Azure, el nombre de la suscripción de Azure y el nombre del recurso de Azure asociado con el servicio QnA Maker en las listas del **paso 2** del portal de QnA Maker. Seleccione el servicio Azure QnA Maker que va a hospedar la base de conocimiento.

    ![Instalación del servicio QnA](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

4. Escriba el nombre de la base de conocimiento y los orígenes de datos de la nueva base de conocimiento.

    ![Establecimiento de los orígenes de datos](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Asigne un **nombre** al servicio. Se admiten nombres duplicados y caracteres especiales.
    - Agregue las direcciones URL de los datos que quiere extraer. Vea más información sobre los tipos de orígenes admitidos [aquí](../Concepts/data-sources-supported.md).
    - Cargue los archivos de los datos que quiere extraer. Consulte la [información sobre precios](https://aka.ms/qnamaker-pricing) para ver cuántos documentos puede agregar.
    - Si quiere agregar manualmente preguntas y respuestas, puede omitir el **paso 4** que se muestra en la imagen anterior.

5. Agregue **charlas** a la base de conocimiento. Opte por agregar compatibilidad de charla al bot al elegir entre una de las tres personalidades predefinidas. 

    <!-- TBD: add back in when chit chat how-to is merged
    ![Add chit-chat to KB ](../media/qnamaker-how-to-chitchat/create-kb-chit-chat.png)
    -->

6. Seleccione **Create your KB** (Crear la base de conocimiento).

    ![Crear una base de conocimiento](../media/qnamaker-how-to-create-kb/create-kb.png)

7. Se tarda unos minutos en extraer los datos.

    ![Extracción](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

8. Si la base de conocimiento se ha creado correctamente, se le redirige a la página **Base de conocimiento**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Agregar charlas personales](./chit-chat-knowledge-base.md)
