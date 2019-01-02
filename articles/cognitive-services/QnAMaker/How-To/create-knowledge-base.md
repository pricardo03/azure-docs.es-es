---
title: Creación de una base de conocimiento
titleSuffix: QnA Maker - Azure Cognitive Services
description: Utilice el portal de QnA Maker para crear una base de conocimiento con adición de preguntas y respuestas de charla. Esto hace que la aplicación sea más atractiva. Agregue un conjunto rellenado previamente de las principales charlas a la base de conocimiento como punto de partida para las charlas del bot y le ahorrará el tiempo y el costo que supone escribirlas desde cero.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 12/11/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 4a4f3d44454e7851f74bbeb072a9d4dcd600a6ef
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53338131"
---
# <a name="quickstart-create-a-knowledge-base-using-the-qna-maker-portal"></a>Inicio rápido: Creación de una base de conocimiento mediante el portal de QnA Maker

QnA Maker facilita la incorporación de orígenes de datos existentes a la hora de crear una base de conocimiento. Puede crear una nueva base de conocimiento de QnA Maker a partir de los siguientes tipos de documentos:

<!-- added for scanability -->
* Páginas de preguntas más frecuentes
* Manuales de productos
* Documentos estructurados

Incluya una personalidad de charla para hacer que sus conocimientos sean más atractivos para los usuarios.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 

## <a name="create-a-new-knowledge-base"></a>Crear una base de conocimiento

1. Inicie sesión en el [portal de QnA Maker](https://qnamaker.ai) con las credenciales de Azure y seleccione **Create a knowledge base** (Crear una base de conocimientos).

1. Si aún no ha creado un servicio QnA Maker, haga clic en **Create a QnA service** (Crear un servicio QnA). 

1. Seleccione el inquilino de Azure, el nombre de la suscripción de Azure y el nombre del recurso de Azure asociado con el servicio QnA Maker en las listas del **paso 2** del portal de QnA Maker. Seleccione el servicio Azure QnA Maker que va a hospedar la base de conocimiento.

    ![Instalación del servicio QnA](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

1. Escriba el nombre de la base de conocimiento y los orígenes de datos de la nueva base de conocimiento.

    ![Establecimiento de los orígenes de datos](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Asigne un **nombre** al servicio. Se admiten nombres duplicados y caracteres especiales.
    - Agregue las direcciones URL de los datos que quiere extraer. Vea más información sobre los tipos de orígenes admitidos [aquí](../Concepts/data-sources-supported.md).
    - Cargue los archivos de los datos que quiere extraer. Consulte la [información sobre precios](https://aka.ms/qnamaker-pricing) para ver cuántos documentos puede agregar.
    - Si quiere agregar manualmente preguntas y respuestas, puede omitir el **paso 4** que se muestra en la imagen anterior.

1. Agregue **charlas** a la base de conocimiento. Opte por agregar compatibilidad de charla al bot al elegir entre una de las tres personalidades. 

    ![Adición de preguntas y respuestas de charla a la base de conocimiento ](../media/qnamaker-how-to-create-kb/create-kb-chit-chat.png)

1. Seleccione **Create your KB** (Crear la base de conocimiento).

    ![Crear una base de conocimiento](../media/qnamaker-how-to-create-kb/create-kb.png)

1. Se tarda unos minutos en extraer los datos.

    ![Extracción](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

1. Si la base de conocimiento se ha creado correctamente, se le redirige a la página **Base de conocimiento**.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con la base de conocimiento, elimínela en el portal de QnA Maker.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Agregar charlas personales](./chit-chat-knowledge-base.md)
