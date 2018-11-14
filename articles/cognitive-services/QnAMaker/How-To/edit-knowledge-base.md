---
title: 'Edición de una base de conocimiento: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker le permite administrar el contenido de la base de conocimiento, proporcionando una experiencia de edición sencilla.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 11/06/2018
ms.author: tulasim
ms.openlocfilehash: adcefe8fed927aca2533ea811bac56f0b92288de
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51279768"
---
# <a name="edit-a-knowledge-base"></a>Editar una base de conocimiento

QnA Maker le permite administrar el contenido de la base de conocimiento, proporcionando una experiencia de edición sencilla.

## <a name="edit-your-knowledge-base-content"></a>Edición del contenido de una base de conocimiento

1.  Seleccione **Mis bases de conocimiento** en la barra de navegación superior. 

    Puede observar que todos los servicios creados o compartidos con usted aparecen ordenados de forma descendente en función de la fecha de la **última modificación**.

    ![Mis bases de conocimiento](../media/qnamaker-how-to-edit-kb/my-kbs.png)

2. Seleccione una base de conocimiento determinada para editarla.
 
3. Haga clic en **Configuración**.

   Aquí puede editar el campo obligatorio Nombre de servicio.
  
   Puede agregar nuevas direcciones URL para agregar nuevo contenido de preguntas más frecuentes para la base de conocimiento haciendo clic en el vínculo **Administrar base de conocimiento -> + Agregar dirección URL**.
   
   Puede eliminar las direcciones URL existentes haciendo clic en **icono de eliminar**.
   
   Si quiere que la base de conocimiento rastree el contenido más reciente de las direcciones URL existentes, marque la casilla de verificación con el nombre **Actualizar**. Esto actualizará la base de conocimiento con el contenido más reciente de la dirección URL.
   
Puede agregar un documento de archivo compatible para formar parte de la base de conocimiento haciendo clic en **Administrar base de conocimiento -> + Agregar archivo**.

También puede importar cualquier base de conocimiento existente haciendo clic en el botón **Importar base de conocimiento**. 
   
La actualización de base de conocimiento depende del **plan de tarifa de administración** que se va a usar al crear el servicio QnA Maker asociado con su base de conocimiento. También puede actualizar el nivel de administración de Azure Portal, si es necesario.

4. Una vez realizados los cambios en la base de conocimiento, haga clic en **Guardar y entrenar** en la esquina superior derecha de la página para persistir los cambios.    

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

## <a name="delete-knowledge-bases"></a>Eliminación de bases de conocimiento

La eliminación de una base de conocimiento (KB) es una operación permanente. No se puede deshacer. Antes de eliminar una base de conocimiento, debe exportarla desde la página **Configuración** del portal de QnA Maker. 

Si comparte la KB con [colaboradores](collaborate-knowledge-base.md) y, a continuación, la elimina, todos perderán el acceso a la KB. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Colaborar en una base de conocimiento](./collaborate-knowledge-base.md)
