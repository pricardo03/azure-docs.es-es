---
title: Conectarse a Outlook.com
description: Administración del correo electrónico, los contactos y los calendarios con las API de REST de Outlook.com y Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/18/2016
tags: connectors
ms.openlocfilehash: 750efc2cb928bf127c4f3c68d5a58c5f52ca7d51
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789379"
---
# <a name="manage-email-calendars-and-contacts-in-outlookcom-with-azure-logic-apps"></a>Administración del correo electrónico, los contactos y los calendarios en Outlook.com con Azure Logic Apps

En este artículo se muestra cómo puede crear y administrar su cuenta de Outlook.com dentro de una aplicación lógica con el conector de Box. De este modo, puede crear aplicaciones lógicas que automatizan tareas y flujos de trabajo para la cuenta de Outlook.com; por ejemplo:

* Enviar correo electrónico. 
* Programar reuniones.
* Agregar contactos. 

Si no está familiarizado con las aplicaciones lógicas, vea [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Requisitos previos

* Una [cuenta de Outlook.com](https://outlook.live.com/owa/)

* Una suscripción de Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/). 

* La aplicación lógica donde quiere acceder a su cuenta de Outlook.com. Para iniciar la aplicación lógica con un desencadenador de Outlook, necesita una [aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-outlookcom"></a>Conectarse a Outlook.com

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Connect to Outlook.com](../../includes/connectors-create-api-outlook.md)]

## <a name="connector-reference"></a>Referencia de conectores

Para obtener datos técnica, como los desencadenadores, las acciones y los límites, tal como lo describe el archivo Swagger del conector, consulte la [página de referencia del conector](/connectors/outlook/). 

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)