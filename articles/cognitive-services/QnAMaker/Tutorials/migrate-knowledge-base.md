---
title: Migración de bases de conocimiento (QnA Maker)
titleSuffix: Azure Cognitive Services
description: Mueva una base de conocimiento creada con QnA Maker a otra base de conocimiento.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/06/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 73f355a6e8c9373a5c31dd7cfebd4455aa324302
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55809750"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Migrar una base de conocimiento mediante la exportación e importación

La migración de una base de conocimiento requiere la exportación de una base de conocimiento y la posterior importación a otra. 

## <a name="prerequisites"></a>Requisitos previos

* Crear una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de comenzar.
* Configurar un [servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md) nuevo

## <a name="migrate-a-knowledge-base-from-qna-maker"></a>Migración de una base de conocimiento desde QnA Maker
1. Inicie sesión en el [portal de QnA Maker](https://qnamaker.ai).
1. Seleccione la base de conocimiento que desea migrar.

1. En la página **Settings** (Configuración), seleccione **Export knowledge base** (Exportar base de conocimiento) para descargar un archivo .tsv que contenga el contenido de su base de conocimiento (preguntas, respuestas, metadatos y los nombres del origen de datos del que se han extraído).

1. Seleccione **Create a knowledge base** (Crear una base de conocimiento) en el menú superior y cree una base de conocimiento vacía. 

    ![Establecimiento de los orígenes de datos](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Asigne un **nombre** al servicio. Se admiten nombres duplicados y también caracteres especiales.

1. Seleccione **Crear**.

    ![Crear una base de conocimiento](../media/qnamaker-how-to-create-kb/create-kb.png)

1. En esta base de conocimiento nueva, abra la pestaña **Settings** (Configuración) y seleccione **Import knowledge base** (Importar base de conocimiento). Esto importa las preguntas, respuestas y metadatos, y conserva los nombres de origen de datos desde el que se extrajeron.

   ![Importación de la base de conocimiento](../media/qnamaker-how-to-migrate-kb/Import.png)

1. **Pruebe** la base de conocimiento nueva mediante el panel de pruebas. Vea cómo [probar la base de conocimiento](../How-To/test-knowledge-base.md).
1. **Publique** la base de conocimiento. Vea cómo [publicar la base de conocimiento](../How-To/publish-knowledge-base.md).
1. Use el punto de conexión del código de la aplicación o del bot. Vea aquí cómo [crear un bot de QnA](../Tutorials/create-qna-bot.md).

    ![Valores de QnA Maker](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

    En este punto, todo el contenido de la base de conocimiento (preguntas, respuestas y metadatos) junto con los nombres de los archivos de código fuente y las direcciones URL, se importan a la base de conocimiento nueva. 

## <a name="chat-logs-and-alterations"></a>Modificaciones y registros de chat
Las modificaciones (sinónimos) no se importan de forma automática. Use las [API de la V2](https://aka.ms/qnamaker-v2-apis) para exportar las modificaciones del conocimiento anterior y las [API de la V4](https://aka.ms/qnamaker-v4-apis) para mover las modificaciones a nueva base de conocimiento.

No hay forma de migrar registros de chat, ya que la base de conocimiento nueva usa Application Insights para almacenar registros de chat. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Editar una base de conocimiento](../How-To/edit-knowledge-base.md)
