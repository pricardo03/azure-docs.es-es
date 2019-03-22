---
title: Conéctese a Dropbox - Azure Logic Apps
description: Cargar y administrar archivos con las API REST de Dropbox y Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 03/01/2019
tags: connectors
ms.openlocfilehash: 5a1bfe8ca38fc23f09b13195fb8ca5bd443a4afd
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58314423"
---
# <a name="upload-and-manage-files-in-dropbox-by-using-azure-logic-apps"></a>Cargar y administrar archivos de Dropbox mediante el uso de Azure Logic Apps

Con el conector de Dropbox y Azure Logic Apps, puede crear flujos de trabajo automatizados que cargar y administran archivos en su cuenta de Dropbox. 

Este artículo muestra cómo conectarse a Dropbox desde la aplicación lógica y, a continuación, agregue la lista desplegable **cuando se crea un archivo** desencadenador y la lista desplegable **obtener contenido del archivo mediante ruta de acceso** acción.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>.

* Un [cuenta de Dropbox](https://www.dropbox.com/), que puede registrarse de forma gratuita. Las credenciales de cuenta son necesarias para crear una conexión entre la aplicación lógica y la cuenta de Dropbox.

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md). En este ejemplo, necesita una aplicación lógica en blanco.

## <a name="add-trigger"></a>Incorporación de un desencadenador

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. En el cuadro de búsqueda, elija **Todas**. En el cuadro de búsqueda, escriba "dropbox" para el filtro.
En la lista de desencadenadores, seleccione este desencadenador: **Cuando se crea un archivo**

   ![Selección del desencadenador de Dropbox](media/connectors-create-api-dropbox/select-dropbox-trigger.png)

1. Inicie sesión con sus credenciales de cuenta de Dropbox y autorice el acceso a los datos de Dropbox para Azure Logic Apps.

1. Proporcione la información necesaria para el desencadenador. 

   En este ejemplo, seleccione la carpeta donde desea realizar un seguimiento de la creación de archivos. Para examinar las carpetas, elija el icono de carpeta junto a la **carpeta** cuadro.

## <a name="add-action"></a>Agregar una acción

Ahora, agregue una acción que obtiene el contenido de cualquier archivo nuevo.

1. En el desencadenador, elija **Next step** (Paso siguiente). 

1. En el cuadro de búsqueda, elija **Todas**. En el cuadro de búsqueda, escriba "dropbox" para el filtro.
En la lista de acciones, seleccione esta acción: **Obtener contenido de archivo mediante la ruta de acceso**

1. Si ya no ha autorizado Azure Logic Apps para acceder a Dropbox, ahora autorizar el acceso.

1. Para ir a la ruta de acceso de archivo que desea usar, junto a la **ruta de acceso de archivo** , seleccione el botón de puntos suspensivos (**...** ) botón. 

   También puede hacer clic dentro de la **ruta de acceso de archivo** cuadro y, en la lista de contenido dinámico, seleccione **ruta de acceso del archivo**, cuyo valor está disponible como salida del desencadenador que agregó en la sección anterior.

1. Cuando haya terminado, guarde la aplicación lógica.

1. Para desencadenar la aplicación lógica, cree un nuevo archivo en Dropbox.

## <a name="connector-reference"></a>Referencia de conectores

Para obtener detalles técnicos, como desencadenadores, acciones y los límites, como se describe en OpenAPI del conector (anteriormente Swagger) de archivos, consulte el [página de referencia del conector](/connectors/dropbox/).

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
