---
title: 'Publicación de una base de conocimiento: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Publicación de una base de conocimientos
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: e9dbeacfb0df98c6b8f084c263690c05fe966cdc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381727"
---
# <a name="publish-a-knowledge-base"></a>Publicación de una base de conocimientos

La publicación de la base de conocimiento es el último paso para hacer disponible la base de conocimiento como un punto de conexión pregunta-respuesta. 

Cuando se publica una base de conocimiento, el contenido de QnA de la base de conocimiento se mueve desde el índice de prueba a un índice de producción en Azure Search.

![Publicación de un índice de prueba de producción](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

## <a name="publish-a-knowledge-base"></a>Publicación de una base de conocimientos

1. Una vez haya realizado los cambios de la base de conocimiento, seleccione **Publicar** en la barra de navegación superior. Puede publicar hasta el número asignado de bases de conocimiento para Azure Search. 

    ![Publicación de una base de conocimiento](../media/qnamaker-how-to-publish-kb/publish.png)

2. Vuelva a seleccionar **Publicar** para ver los detalles del punto de conexión que se pueden usar en la aplicación o en el código del bot.

    ![Publicación de una base de conocimiento](../media/qnamaker-how-to-publish-kb/publish-success.png)
    
## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Obtener análisis en la base de conocimiento](./get-analytics-knowledge-base.md)
