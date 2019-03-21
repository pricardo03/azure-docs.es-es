---
title: Publicación de una base de conocimientos
titleSuffix: QnA Maker API - Azure Cognitive Services
description: La publicación de la base de conocimiento con el servicio QnA Maker API es el último paso para disponer de la base de conocimiento como un punto de conexión de responder a la pregunta. Cuando se publica una base de conocimiento, el contenido de QnA de la base de conocimiento se mueve desde el índice de prueba a un índice de producción en Azure Search.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/11/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: c65654e7d6b2e66a07116ea9555ed316ace88912
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58091960"
---
# <a name="publish-a-knowledge-base-using-the-qna-maker-api-service-portal"></a>Publicar una base de conocimiento con el portal de servicios de QnA Maker API

La publicación de la base de conocimiento con el servicio QnA Maker API es el último paso para disponer de la base de conocimiento como un punto de conexión de responder a la pregunta. 

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
