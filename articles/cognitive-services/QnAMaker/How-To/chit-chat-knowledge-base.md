---
title: Adición de charla a una base de conocimiento de QnA Maker
titleSuffix: Azure Cognitive Services
description: La incorporación de charlas personales al bot lo hace más atractivo y conversacional al crear la base de conocimiento. QnA Maker permite agregar fácilmente un conjunto previamente rellenado de las principales charlas a la base de conocimiento.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 05/07/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 08d09680ee1797e73666c185f4430c7ef3079477
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153676"
---
# <a name="add-chit-chat-to-a-knowledge-base"></a>Adición de una charla a la base de conocimiento

La incorporación de charlas al bot lo hace más atractivo y conversacional. La característica de charla de QnA Maker permite agregar fácilmente un conjunto previamente rellenado de las principales charlas a la base de conocimiento (KB). Esto puede ser un punto de partida para la personalidad del bot y permite ahorrar el tiempo y el costo que supone escribirlas desde cero.  

Este conjunto de datos tiene aproximadamente 100 escenarios de chit chat en la voz de varios roles, como Professional, sencillo y Witty. Elija el rol que más se parezca a la voz del bot. Dada una consulta de usuario, QnA Maker intenta hacerla coincidir con la QnA de charla conocida más parecida.  

Algunos ejemplos de las personalidades diferentes están por debajo. Puede ver todos los conjuntos de datos de personalidad junto con los detalles de las personalidades [aquí](https://github.com/Microsoft/BotBuilder-PersonalityChat/tree/master/CSharp/Datasets).

<!-- added quotes so acrolinx doesn't score these sentences -->
|Consulta de usuario|Profesional|Descriptivo|Astuto|
|--|--|--|--|
|`You are awesome`|`I aim to serve.`|`Aw, I'm blushing.`|`Flattery. I like it.`|
|`Are you hungry?`|`I don't need to eat.`|`I only do food for thought.`|`Eating would require a lot of things I don't have. Like a digestive system. And silverware.`|
|`Sing a song`|`I'm afraid I'm not musically inclined.`|`La la la, tra la la. I'm awesome at this.`|`Those who can, do. Those who can't, don't sing.`|
|`Will you marry me?`|`I think it's best if we stick to a professional relationship.`|`Definitely didn't see that coming!`|`Sure. Take me to city hall. See what happens.`|



> [!NOTE]
> En la actualidad, la charla solo está disponible en inglés. 

## <a name="add-chit-chat-during-kb-creation"></a>Adición de charlas durante la creación de la KB
Durante la creación de la base de conocimiento, después de agregar las direcciones URL y archivos de origen, hay una opción para agregar charlas. Elija la personalidad que quiera como base de la charla. Si no quiere agregar charlas o si ya tiene compatibilidad con la charla en los orígenes de datos, seleccione **None** (Ninguno). 
   
![Adición de charlas durante la creación](../media/qnamaker-how-to-chit-chat/create-kb-chit-chat.png)

## <a name="add-chit-chat-to-an-existing-kb"></a>Adición de charlas a una base de conocimiento existente
Seleccione la base de conocimiento y vaya hasta la página **Settings** (Configuración). Hay un vínculo a todos los conjuntos de datos de charla en el formato **.tsv** correspondiente. Descargue la personalidad que quiera y, después, cárguela como un origen de archivo. Asegúrese de no modificar el formato o los metadatos al descargar y cargar el archivo. 
  
![Adición de charlas a una base de conocimiento existente](../media/qnamaker-how-to-chit-chat/add-chit-chat-dataset.png)

## <a name="edit-your-chit-chat-questions-and-answers"></a>Edición de las preguntas y respuestas de la charla
Cuando edite la base de conocimiento, verá un nuevo origen para la charla, según la personalidad que haya seleccionado. Ahora puede agregar preguntas modificadas o editar las respuestas, como con cualquier otro origen. 

![Edición de preguntas y respuestas de la charla](../media/qnamaker-how-to-chit-chat/edit-chit-chat.png)

Para ver los metadatos, seleccione **opciones de vista** en la barra de herramientas, a continuación, seleccione **muestran metadatos**.

## <a name="add-additional-chit-chat-questions-and-answers"></a>Adición de preguntas y respuestas adicionales de la charla
Puede agregar nuevas preguntas y respuestas de la charla que no estén en el conjunto predefinido. Asegúrese de que no va a duplicar un par de QnA que ya esté presente en el conjunto de la charla. Cuando se agregan nuevas preguntas y respuestas de charla, se agregan al origen **Editorial**. Para asegurarse de que el clasificador comprende que se trata de charla, agregue el par de clave y valor de metadatos "Editorial: chit-chat", como se muestra en la imagen siguiente:
   
![![Add chit-chat QnAs](../media/qnamaker-how-to-chit-chat/add-new-chit-chat.png)](../media/qnamaker-how-to-chit-chat/add-new-chit-chat.png#lightbox)

## <a name="delete-chit-chat-from-an-existing-kb"></a>Eliminación de la charla de una base de conocimiento existente
Seleccione la base de conocimiento y vaya hasta la página **Settings** (Configuración). El origen de charla específico aparece como un archivo, con el nombre de la personalidad seleccionado. Puede eliminarlo como un archivo de origen.

![Eliminación de la charla de la base de conocimiento](../media/qnamaker-how-to-chit-chat/delete-chit-chat.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Importación de una base de conocimiento](../Tutorials/migrate-knowledge-base.md)

## <a name="see-also"></a>Vea también 

[Introducción de QnA Maker](../Overview/overview.md)
