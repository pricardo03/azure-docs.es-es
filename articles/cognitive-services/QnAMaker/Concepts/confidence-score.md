---
title: 'Puntuación de confianza: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Explicación de la puntuación de confianza
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: c97bdb7e57275ebd1893bc28248c4ecc6b35c153
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381798"
---
# <a name="confidence-score"></a>Puntuación de confianza

Una puntuación de confianza indica el grado de coincidencia entre la pregunta del usuario y la respuesta devuelta.

Cuando una consulta de usuario se compara con el contenido de la knowledge base, es posible que se devuelva más de una respuesta. Las respuestas se devuelven en orden de clasificación de mayor a menor puntuación de confianza.

La puntuación de confianza va de 0 a 100.

|Valor de la puntuación|Confianza|
|--|--|
|100|Una coincidencia exacta de la consulta del usuario y una pregunta de KB|
|90|Confianza alta: la mayoría de las palabras coinciden.|
|40-60|Confianza razonable: las palabras importantes coinciden.|
|10|Confianza baja: las palabras importantes no coinciden.|
|0|Ninguna palabra coincidente|


## <a name="similar-confidence-scores"></a>Puntuaciones de confianza similares
Si varias respuestas tienen una puntuación de confianza similar, es probable que la consulta fuera demasiado genérica y, por tanto, coincidiera con la misma probabilidad con varias respuestas. Intente estructurar mejor sus preguntas y respuestas para que todas las entidades QnA tengan una intención distinta.


## <a name="improving-confidence-scores"></a>Mejora de las puntuaciones de confianza
Para mejorar la puntuación de confianza de una respuesta concreta a una consulta de usuario, puede agregar la consulta del usuario a la base de conocimiento como una pregunta alternativa en la respuesta.
   
## <a name="confidence-score-differences"></a>Diferencias en las puntuaciones de confianza
La puntuación de confianza de una respuesta puede cambiar de manera apenas perceptible entre la prueba y la versión publicada de la base de conocimiento, aunque el contenido sea el mismo. Esto se debe a que el contenido de la prueba y la base de conocimiento publicada se encuentran en distintos índices de Azure Search.

Vea aquí cómo se lleva a cabo la operación de [publicación](../How-To/publish-knowledge-base.md).


## <a name="no-match-found"></a>No se encontraron coincidencias
Si el clasificador no encuentra ninguna buena coincidencia, se devuelve una puntuación de confianza de 0,0 o "None" (Ninguna) y la respuesta predeterminada no es "No good match found in the KB" (No se encontró ninguna buena coincidencia en KB)". Puede invalidar esta respuesta predeterminada en el código de aplicación o bot mediante una llamada al punto de conexión. Como alternativa, también puede establecer la respuesta de invalidación en Azure, lo que cambiará el valor predeterminado para todas las knowledge base implementada en un determinado servicio QnA Maker.

1. Vaya a [Azure Portal](http://portal.azure.com) y navegue hasta el grupo de recursos que representa el servicio QnA Maker que creó.

2. Haga clic para abrir **App Service**.

    ![Acceder a App Service](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Haga clic en **Configuración de la aplicación** y edite el campo **DefaultAnswer** a la respuesta predeterminada deseada. Haga clic en **Save**(Guardar).

    ![Cambiar la respuesta predeterminada](../media/qnamaker-concepts-confidencescore/change-response.png)

4. Reiniciar App Service

    ![Reinicio de App Service QnA Maker](../media/qnamaker-faq/qnamaker-appservice-restart.png)


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Orígenes de datos admitidos](./data-sources-supported.md)

## <a name="see-also"></a>Otras referencias 

[Información general de QnA Maker](../Overview/overview.md)
