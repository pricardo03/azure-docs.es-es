---
title: Migración de bases de conocimiento (QnA Maker)
titleSuffix: Azure Cognitive Services
description: La migración de una base de conocimiento requiere la exportación de una base de conocimiento y la posterior importación a otra.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 04/08/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: eac5e43c69cc09c5945316827a35f729c158d47a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61431249"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Migrar una base de conocimiento mediante la exportación e importación

La migración de una base de conocimiento requiere la exportación de una base de conocimiento y la posterior importación a otra. 

## <a name="prerequisites"></a>Requisitos previos

* Cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de comenzar.
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
1. **Publique** la base de conocimiento. Vea cómo [publicar la base de conocimiento](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base).
1. Use el punto de conexión del código de la aplicación o del bot. Vea aquí cómo [crear un bot de QnA](../Tutorials/create-qna-bot.md).

    ![Valores de QnA Maker](../media/qnamaker-how-to-migrate-kb/qnamaker-settings-kbid-key.png)

    En este punto, todo el contenido de la base de conocimiento (preguntas, respuestas y metadatos) junto con los nombres de los archivos de código fuente y las direcciones URL, se importan a la base de conocimiento nueva. 

## <a name="chat-logs-and-alterations"></a>Modificaciones y registros de chat
Las alteraciones (sinónimos) que no distinguen mayúsculas de minúsculas no se importan de forma automática. Use las [API de la V2](https://aka.ms/qnamaker-v2-apis) para exportar las modificaciones del conocimiento anterior y las [API de la V4](https://aka.ms/qnamaker-v4-apis) para mover las modificaciones a nueva base de conocimiento.

No hay forma de migrar registros de chat, ya que la base de conocimiento nueva usa Application Insights para almacenar registros de chat. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Editar una base de conocimiento](../How-To/edit-knowledge-base.md)
