---
title: 'Edición de una base de conocimiento: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Edición de una base de conocimiento
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: eaa65bf3d257399fceadaa42f0d9ddbbf8afe234
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381819"
---
# <a name="edit-a-knowledge-base"></a>Editar una base de conocimiento

QnA Maker le permite administrar el contenido de la base de conocimiento, proporcionando una experiencia de edición sencilla.

## <a name="edit-your-knowledge-base-content"></a>Edición del contenido de una base de conocimiento

1.  Seleccione **Mis bases de conocimiento** en la barra de navegación superior. 

    Puede observar que todos los servicios creados o compartidos con usted aparecen ordenados de forma descendente en función de la fecha de la **última modificación**.

    ![Mis bases de conocimiento](../media/qnamaker-how-to-edit-kb/my-kbs.png)

2. Seleccione una base de conocimiento determinada para editarla.

3. Una vez realizados los cambios en la base de conocimiento, haga clic en **Guardar y entrenar** en la esquina superior derecha de la página para persistir los cambios.    

    ![Guardar y entrenar](../media/qnamaker-how-to-edit-kb/save-and-train.png)

    >[!NOTE]
    Los cambios no persistirán si cierra la página antes de hacer clic en Guardar y entrenar.

## <a name="add-a-qna-pair"></a>Adición de un par de QnA

Seleccione **Adición de un par de QnA** para agregar una fila nueva a la tabla de la base de conocimiento.

![Adición de un par de QnA](../media/qnamaker-how-to-edit-kb/add-qnapair.png)

## <a name="delete-a-qna-pair"></a>Eliminación de un par de QnA

Para eliminar un QnA, haga clic en el icono **eliminar** en el extremo derecho de la fila QnA.

![Eliminación de un par de QnA](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="add-alternate-questions"></a>Agregar preguntas alternativas

Agregue preguntas alternativas a un par de QnA existente para mejorar la probabilidad de encontrar una coincidencia para una consulta de usuario.

![Agregar preguntas alternativas](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="add-metadata"></a>Agregar metadatos


Agregar pares de metadatos seleccionando el icono de filtro

![Agregar metadatos](../media/qnamaker-how-to-edit-kb/add-metadata.png)

> [!TIP]
> Asegúrese de hacer clic periódicamente en Guardar y entrenar en la base de conocimiento después de realizar las ediciones, a fin de evitar perder los cambios.

## <a name="manage-large-knowledge-bases"></a>Administración de bases de conocimiento grandes

1. Los pares de QnA se **agrupan** según el origen de datos del que se han extraído. Puede expandir o contraer el origen de datos.
2. Puede **buscar** en la base de conocimiento si escribe en el cuadro de texto en la parte superior de la tabla de la base de conocimiento. Haga clic en ENTRAR para buscar en el contenido de la pregunta, la respuesta o los metadatos. Haga clic en el icono X para quitar el filtro de búsqueda.
3. La **paginación** permite administrar grandes bases de conocimiento.

    ![Buscar, Paginar, Agrupar](../media/qnamaker-how-to-edit-kb/search-paginate-group.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Colaborar en una base de conocimiento](./collaborate-knowledge-base.md)
