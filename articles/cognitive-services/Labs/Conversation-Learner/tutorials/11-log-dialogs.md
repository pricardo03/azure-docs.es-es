---
title: 'Uso de diálogos de registro con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información acerca de cómo usar diálogos de registro con un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e3a84bfa643ebe74983bcef0d0ea72c701ffa589
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58170503"
---
# <a name="how-to-log-dialogs-in-a-conversation-learner-model"></a>Uso de diálogos de registro con un modelo de Conversation Learner

En este tutorial se muestra cómo se emplean los diálogos de registro para entrenar mejor los modelos de Conversation Learner a partir de las interacciones grabadas con usuarios reales.

## <a name="video"></a>Vídeo

[![Vista previa del tutorial de diálogos de registro](https://aka.ms/cl_Tutorial_v3_LogDialogs_Preview)](https://aka.ms/cl_Tutorial_v3_LogDialogs)

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

y debe haber creado el modelo de tiempo en tutoriales anteriores.

## <a name="details"></a>Detalles
Los diálogos de registro son registros grabados de la interacción de su bot con los usuarios finales. Al aprovechar estos diálogos de registro, puede corregir etiquetas de entidad y selecciones de acciones para mejorar el rendimiento del modelo y del sistema en general.

## <a name="steps"></a>Pasos

En la interfaz de usuario web, haga clic en "Import Tutorials" (Importar tutoriales) y seleccione el modelo denominado "Tutorial-11-LogDialogs".

Este modelo contiene una entidad denominada "city" (ciudad) y acciones diseñadas para responder a preguntas sobre el tiempo en esa ciudad. Se han usado dos diálogos de entrenamiento para entrenar el modelo, por lo que las expectativas de rendimiento son algo bajas. El modelo mejoraría con un entrenamiento adicional y una exposición a interacciones de usuarios reales.

### <a name="create-a-new-conversation"></a>Creación de una conversación

1. En el panel izquierdo, haga clic en "Log Dialogs" (Diálogos de registro) y, después, en el botón "New Log Dialog" (Nuevo diálogo de registro).
2. En el panel del chat, donde aparece "Type your message..." (Escriba su mensaje…), escriba "Previsión meteorológica de Austin".
3. Haga clic en el botón "Done Testing" (Finalizar la prueba).
4. Haga clic en el diálogo de registro "Previsión meteorológica de Austin" en la lista.
5. Haga clic en la expresión "Previsión meteorológica de Austin" en el panel del chat.
6. Haga clic en "Austin" y después en "city" (ciudad) en la lista de entidades.
7. Haga clic en el botón "Submit Changes" (Enviar cambios).
    - Este cambio en el valor de la entidad provoca cambios en la conversación ya que tenemos nuevos valores de entidad en la memoria. Es probable que las acciones posteriores hayan dejado de ser válidas, en concreto aquellas que implican la entidad "city" (ciudad).
8. Haga clic en la expresión "¿Qué ciudad?" en el panel del chat.
9. Seleccione la respuesta "Parece que está soleado en Austin".
10. Haga clic en el botón "Save As Train Dialog" (Guardar como diálogo de entrenamiento).
    - El entrenamiento empieza de inmediato.

![](../media/T11_logdialog.png)

Un último apunte. En función de las necesidades empresariales, la característica de registro de la conversación puede desactivarse; para ello, vaya a la configuración y desactive la casilla "Log Conversations" (Registrar conversaciones).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Devolución de llamada de detección de entidades](./12-entity-detection-callback.md)
