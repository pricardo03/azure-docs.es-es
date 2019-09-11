---
title: 'Creación de una base de conocimiento: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Use el portal de servicios de QnA Maker API para crear una base de conocimiento con charla. Esto hace que la aplicación sea más atractiva. Agregue un conjunto rellenado previamente de las principales charlas a la base de conocimiento como punto de partida para las charlas del bot y le ahorrará el tiempo y el costo que supone escribirlas desde cero.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 08/30/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 322a1d25ed434d8be674288b1b13d6ecf961590b
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70193565"
---
# <a name="quickstart-create-a-knowledge-base-using-the-qna-maker-api-service-portal"></a>Inicio rápido: Creación de una base de conocimiento mediante el portal de servicios de QnA Maker API

El portal de servicios de QnA Maker API facilita la incorporación de orígenes de datos existentes al crear una base de conocimiento. Puede crear una nueva base de conocimiento de QnA Maker a partir de los siguientes tipos de documentos:

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

1. Asigne un **nombre** como `my first kb` al servicio. Se admiten nombres duplicados y caracteres especiales.

1. Agregue la página de solución de problemas de QnA Maker como dirección URL: `https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/troubleshooting` y, luego, seleccione `+ Add URL`. Vea más información sobre los tipos de orígenes admitidos [aquí](../Concepts/data-sources-supported.md). Para este inicio rápido, **no cargue archivos** para los datos que desea extraer. Consulte la [información sobre precios](https://aka.ms/qnamaker-pricing) para ver cuántos documentos puede agregar.

1. Agregue **_charla profesional_** a su base de conocimiento. 

1. Seleccione **Create your KB** (Crear la base de conocimiento).

    ![Crear una base de conocimiento](../media/qnamaker-how-to-create-kb/create-kb.png)

1. Se tarda unos minutos en extraer los datos.

    ![Extracción](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

1. Si la base de conocimiento se ha creado correctamente, se le redirige a la página **Base de conocimiento**.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con la base de conocimiento, elimínela en el portal de QnA Maker.

## <a name="next-steps"></a>Pasos siguientes

Como medidas de ahorro de los costos, puede [compartir](set-up-qnamaker-service-azure.md#share-existing-services-with-qna-maker) algunos pero no todos los recursos de Azure creados para QnA Maker.

> [!div class="nextstepaction"]
> [Adición de preguntas con metadatos](../quickstarts/add-question-metadata-portal.md)
