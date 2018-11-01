---
title: Conectarse a Outlook.com - Azure Logic Apps | Microsoft Docs
description: Administración del correo electrónico, los contactos y los calendarios con las API de REST de Outlook.com y Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.assetid: 87113c85-d158-4dd5-9ed5-5748130003d6
ms.topic: article
ms.date: 08/18/2016
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: fd6836451a73551487b8f97903594154a2efc894
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50228762"
---
# <a name="manage-email-calendars-and-contacts-in-outlookcom-with-azure-logic-apps"></a>Administración del correo electrónico, los contactos y los calendarios en Outlook.com con Azure Logic Apps

En este artículo se muestra cómo puede crear y administrar su cuenta de Outlook.com dentro de una aplicación lógica con el conector de Box. De este modo, puede crear aplicaciones lógicas que automatizan tareas y flujos de trabajo para la cuenta de Outlook.com; por ejemplo:

* Enviar correo electrónico. 
* Programar reuniones.
* Agregar contactos. 

Si no está familiarizado con las aplicaciones lógicas, vea [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Requisitos previos

* Una [cuenta de Outlook.com](https://outlook.live.com/owa/)

* Una suscripción de Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>. 

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