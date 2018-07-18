---
title: 'Cómo importar una base de conocimiento: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Cómo importar una base de conocimiento
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: ce8f98f9bdb37d5f326e942fe5b5e815e5272c56
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "35383162"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Migrar una base de conocimiento mediante la exportación e importación
QnA Maker anunció la disponibilidad general el 7 de mayo de 2018, en la conferencia \\\build\. QnA Maker GA tiene una nueva arquitectura basada en Azure. Las bases de conocimiento que se crean con la versión preliminar gratuita de QnA Maker se tienen que migrar a QnA Maker GA (disponibilidad general). QnA Maker Preview quedará en desuso en noviembre de 2018. Para obtener más información sobre los cambios en QnA Maker GA, vea la [entrada de blog](https://aka.ms/qnamakerga-blog) del anuncio de QnA Maker GA.

QnA Maker ahora tiene un [modelo de precios](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/qna-maker/).

requisitos previos
> [!div class="checklist"]
> * Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
> * Configure un [servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md) nuevo.

## <a name="migrate-a-knowledge-base-from-qna-maker-preview-portal"></a>Migrar una base de conocimiento desde el portal de QnA Maker Preview
1. Vaya al [portal de QnA Maker Preview](https://aka.ms/qnamaker-old-portal
) y haga clic en **My services** (Mis servicios).
2. Haga clic en el icono de edición para seleccionar la base de conocimiento que quiera migrar.

    ![Edición de la base de conocimiento](../media/qnamaker-how-to-migrate-kb/preview-editkb.png)

3. Haga clic en **Download knowledge base** (Descargar la base de conocimiento) para descargar un archivo .tsv en el que se incluye el contenido de la base de conocimiento: preguntas, respuestas, metadatos, y los nombres del origen de datos del que se han extraído.

    ![Descarga de la base de conocimiento](../media/qnamaker-how-to-migrate-kb/preview-download.png)

4. Inicie sesión en el [portal de QnA Maker](https://qnamaker.ai) con las credenciales de Azure y haga clic en **Create new service** (Crear servicio).

    ![Crear una base de conocimiento ](../media/qnamaker-how-to-create-kb/create-new-service.png)
    
5. Si aún no ha creado un servicio QnA Maker, haga clic en **Create a QnA service** (Crear un servicio QnA). En caso contrario, elija un servicio QnA Maker en las listas desplegables del paso 2. Seleccione el servicio QnA Maker que va a hospedar la base de conocimiento.

    ![Instalación del servicio QnA](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

6. Crear una base de conocimiento vacía 

    ![Establecimiento de los orígenes de datos](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Asigne un **nombre** al servicio. Se admiten nombres duplicados y también caracteres especiales.
    - Omita la carga de archivos o direcciones URL como quiera para usar los datos de la base de conocimiento de versión preliminar. Por el momento, creará una base de conocimiento vacía.

7. Seleccione **Crear**.

    ![Crear una base de conocimiento](../media/qnamaker-how-to-create-kb/create-kb.png)

8. En esta base de conocimiento nueva, abra la pestaña **Settings** (Configuración) y haga clic en **Import knowledge base** (Importar base de conocimiento). Esto importa las preguntas, respuestas y metadatos, y conserva los nombres de origen de datos desde el que se extrajeron.

   ![Importación de la base de conocimiento](../media/qnamaker-how-to-migrate-kb/Import.png)

9. **Pruebe** la base de conocimiento nueva mediante el panel de pruebas. Vea cómo [probar la base de conocimiento](../How-To/test-knowledge-base.md).
10. **Publique** la base de conocimiento. Vea cómo [publicar la base de conocimiento](../How-To/publish-knowledge-base.md).
11. Use el punto de conexión siguiente en el código de la aplicación o el bot. Vea aquí cómo [crear un bot de QnA](../Tutorials/create-qna-bot.md).

    ![Valores de QnA Maker](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

En este punto, todo el contenido de la base de conocimiento (preguntas, respuestas y metadatos) junto con los nombres de los archivos de código fuente y las direcciones URL, se importan a la base de conocimiento nueva. 

## <a name="chatlogs-and-alterations"></a>Registros de conversación y modificaciones
Las modificaciones (sinónimos) no se importan de forma automática. Use las [API V2](https://aka.ms/qnamaker-v2-apis) para exportar las modificaciones de la pila de versión preliminar y las [API V4](https://aka.ms/qnamaker-v4-apis) para reemplazar en la nueva pila.

No hay ninguna manera de migrar los registros de conversación, ya que en la pila nueva se usa Application Insights para almacenarlos. Pero puede descargar los registros de conversación desde el [portal de la versión preliminar](https://aka.ms/qnamaker-old-portal).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Editar una base de conocimiento](../How-To/edit-knowledge-base.md)
