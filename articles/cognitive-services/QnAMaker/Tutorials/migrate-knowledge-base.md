---
title: Migración de bases de conocimiento (QnA Maker)
titleSuffix: Azure Cognitive Services
description: La migración de una base de conocimiento requiere la exportación de una base de conocimiento y la posterior importación a otra.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/28/2020
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 942932c229ace82a0bf66da7a5421f936b028088
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78302566"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Migrar una base de conocimiento mediante la exportación e importación

La migración de una base de conocimiento requiere la exportación de una base de conocimiento y la posterior importación a otra.

## <a name="prerequisites"></a>Prerrequisitos

* Cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de comenzar.
* Configurar un [servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md) nuevo

## <a name="migrate-a-knowledge-base-from-qna-maker"></a>Migración de una base de conocimiento desde QnA Maker
1. Inicie sesión en el [portal de QnA Maker](https://qnamaker.ai).
1. Seleccione la base de conocimiento de origen que quiere migrar.

1. En la página **Settings** (Configuración), seleccione **Export knowledge base** (Exportar base de conocimiento) para descargar un archivo .tsv que incluya el contenido de la base de conocimiento de origen (preguntas, respuestas, metadatos, avisos de seguimiento) y los nombres del origen de datos del que se han extraído.

1. Seleccione **Create a knowledge base** (Crear una base de conocimiento) en el menú superior y cree una base de conocimiento _vacía_. Está vacía porque, cuando se crea, no se agregan direcciones URL ni archivos, sino que lo hacen durante el paso de importación, después de la creación.

    Configure la base de conocimiento. Establezca solo el nombre de la nueva base de conocimiento. Se admiten nombres duplicados y también caracteres especiales.

    No seleccione nada en el paso 4, ya que esos valores se sobrescribirán al importar el archivo.

1. En el paso 5, seleccione **Create** (Crear).

1. En esta base de conocimiento nueva, abra la pestaña **Settings** (Configuración) y seleccione **Import knowledge base** (Importar base de conocimiento). Esta acción importa las preguntas, las respuestas, los metadatos y los avisos de seguimiento, y conserva los nombres del origen de datos del que se extrajeron.

   > [!div class="mx-imgBorder"]
   > [![Importación de la base de conocimiento](../media/qnamaker-how-to-migrate-kb/Import.png)](../media/qnamaker-how-to-migrate-kb/Import.png#lightbox)

1. **Pruebe** la base de conocimiento nueva mediante el panel de pruebas. Vea cómo [probar la base de conocimiento](../How-To/test-knowledge-base.md).

1. **Publique** la base de conocimiento y cree un bot de chat. Vea cómo [publicar la base de conocimiento](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base).

## <a name="chat-logs-and-alterations"></a>Modificaciones y registros de chat
Las alteraciones (sinónimos) que no distinguen mayúsculas de minúsculas no se importan de forma automática. Use las [API V4](https://go.microsoft.com/fwlink/?linkid=2092179) para mover las modificaciones en la nueva base de conocimiento.

No hay forma de migrar registros de chat, ya que la base de conocimiento nueva usa Application Insights para almacenar registros de chat.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Editar una base de conocimiento](../How-To/edit-knowledge-base.md)
