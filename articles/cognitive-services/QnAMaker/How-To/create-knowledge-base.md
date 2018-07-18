---
title: Cómo crear una base de conocimiento - QnA Maker - Azure Cognitive Services | Microsoft Docs
description: Cómo crear una base de conocimiento
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 93b64948ecc52feeb0f862f2b76ea898dce2333a
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "35383157"
---
# <a name="create-a-knowledge-base"></a>Creación de una base de conocimientos

Con QnA Maker, resulta muy fácil incorporar los orígenes de datos existentes para crear una base de conocimiento. Puede crear una nueva base de conocimiento de QnA Maker a partir de páginas de preguntas más frecuentes, manuales de productos o documentos estructurados, puede agregarlos editorialmente.

## <a name="steps"></a>Pasos

1. Para comenzar, inicie sesión en el [portal de QnA Maker](https://qnamaker.ai) con sus credenciales de Azure y haga clic en **Create new service** (Crear nuevo servicio).

    ![Crear una base de conocimiento ](../media/qnamaker-how-to-create-kb/create-new-service.png)

2. Si aún no ha creado un servicio QnA Maker, seleccione **Create a QnA service** (Crear un servicio QnA). En caso contrario, elija un servicio QnA Maker en las listas desplegables del paso 2. Seleccione el servicio QnA Maker que va a hospedar la base de conocimiento.

    ![Instalación del servicio de QnA](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

3. Especifique la información siguiente para crear la base de conocimiento.

    ![Establecimiento de los orígenes de datos](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Asigne un **nombre** al servicio. Se admiten nombres duplicados y también se admiten caracteres especiales.
    - Pegue las direcciones URL de las que se va a extraer la información. Vea más información sobre los tipos de orígenes admitidos [aquí](../Concepts/data-sources-supported.md).
    - También puede cargar archivos de los que se extraerán los datos. Consulte la [información sobre precios](https://aka.ms/qnamaker-pricing
) para ver cuántos documentos puede agregar.
    - Si desea agregar QnA manualmente, puede omitir la vinculación de archivos.

4. Seleccione **Crear**.

    ![Crear una base de conocimiento](../media/qnamaker-how-to-create-kb/create-kb.png)

5. Se tarda unos minutos en extraer los datos.

    ![Extracción](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

6. Si la base de conocimiento se ha creado correctamente, se le redirigirá a la página **Knowledge Base** (Base de conocimiento).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Importación de una base de conocimiento](../Tutorials/migrate-knowledge-base.md)
