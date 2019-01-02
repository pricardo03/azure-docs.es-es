---
title: Publicación de una base de conocimientos
titleSuffix: QnA Maker - Azure Cognitive Services
description: La publicación de la base de conocimiento es el último paso para hacer disponible la base de conocimiento como un punto de conexión pregunta-respuesta. Cuando se publica una base de conocimiento, el contenido de QnA de la base de conocimiento se mueve desde el índice de prueba a un índice de producción en Azure Search.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 12/11/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: deeebd99425bc1ec1ed2ae76ef4852119a9c10ea
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53342449"
---
# <a name="publish-a-knowledge-base-using-the-qna-maker-portal"></a>Publicación de una base de conocimiento mediante el portal de QnA Maker

La publicación de la base de conocimiento es el último paso para hacer disponible la base de conocimiento como un punto de conexión pregunta-respuesta en una aplicación cliente. 

Cuando se publica una base de conocimiento, el contenido de preguntas y respuestas de la base de conocimiento se mueve desde el índice de prueba a un índice de producción en Azure Search.

![Publicación de un índice de prueba de producción](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 

## <a name="publish-a-knowledge-base"></a>Publicación de una base de conocimientos

1. Una vez haya realizado los cambios de la base de conocimiento, seleccione **Publicar** en la barra de navegación superior. Puede publicar hasta el número asignado de bases de conocimiento para Azure Search. 

    ![Publicación de una base de conocimiento](../media/qnamaker-how-to-publish-kb/publish.png)

2. Vuelva a seleccionar **Publicar** para ver los detalles del punto de conexión que se pueden usar en la aplicación o en el código del bot.

    ![Base de conocimientos publicada correctamente](../media/qnamaker-how-to-publish-kb/publish-success.png)
    
## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con la base de conocimiento, elimínela en el portal de QnA Maker.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Obtener análisis en la base de conocimiento](./get-analytics-knowledge-base.md)
