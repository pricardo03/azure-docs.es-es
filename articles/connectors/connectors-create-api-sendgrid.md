---
title: 'Conexión con SendGrid: Azure Logic Apps | Microsoft Docs'
description: Automatización de tareas y flujos de trabajo que envíen correos electrónicos y administren listas de distribución de correo electrónico de SendGrid con Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: bc4f1fc2-824c-4ed7-8de8-e82baff3b746
ms.topic: article
tags: connectors
ms.date: 08/24/2018
ms.openlocfilehash: c8747210a77879d551e323a7c0e46a9ab013fa3f
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42887196"
---
# <a name="send-emails-and-manage-mailing-lists-in-sendgrid-by-using-azure-logic-apps"></a>Envío de correos electrónicos y administración de listas de distribución de correo electrónico en SendGrid con Azure Logic Apps

Con Azure Logic Apps y el conector de SendGrid, puede crear tareas automatizadas y flujos de trabajo que envíen correos electrónicos y administren las listas de destinatarios, por ejemplo, puede:

* Enviar correo electrónico.
* Agregar destinatarios a las listas.
* Obtener, agregar y administrar la supresión global.

Puede usar acciones de SendGrid en las aplicaciones lógicas para realizar estas tareas. También puede tener otras acciones que usen la salida de las acciones de SendGrid. 

Este conector ofrece únicamente las acciones, por lo que, para iniciar la aplicación lógica, use un desencadenador independiente, como puede ser uno de **periodicidad**. Por ejemplo, si agrega destinatarios a las listas con regularidad, puede enviar mensajes de correo electrónico a los destinatarios y las listas mediante el conector de Office 365 Outlook o Outlook.com.
Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>. 

* Una [cuenta SendGrid](https://www.sendgrid.com/) y una [clave de API de SendGrid](https://sendgrid.com/docs/ui/account-and-settings/api-keys/)

   La clave de API autoriza a la aplicación lógica a crear una conexión y acceder a su cuenta de SendGrid.

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* La aplicación lógica donde quiere acceder a su cuenta de SendGrid. Para usar una acción de SendGrid, inicie la aplicación lógica con otro desencadenador, por ejemplo, el de **periodicidad**.

## <a name="connect-to-sendgrid"></a>Conexión con SendGrid

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la aplicación lógica en el Diseñador de aplicaciones lógicas, si aún no lo ha hecho.

1. Elija una ruta de acceso: 

   * En el último paso para agregar una acción, elija **Nuevo paso**. 

     O bien

   * Entre los pasos en los que desee agregar una acción, mueva el puntero sobre la flecha. 
   Elija el signo más (**+**) que aparece y seleccione **Agregar una acción**.

1. En el cuadro de búsqueda, escriba "sendgrid" como filtro. En la lista de acciones, seleccione la que desee.

1. Proporcione un nombre para la conexión. 

1. Escriba la clave de API de SendGrid y, a continuación, elija **Crear**.

1. Proporcione los detalles necesarios para la acción seleccionada y continúe con la creación del flujo de trabajo de la aplicación lógica.

## <a name="connector-reference"></a>Referencia de conectores

Para obtener detalles técnicos sobre desencadenadores, acciones y límites, que se describen en la descripción de OpenAPI (antes Swagger) del conector, consulte la [página de referencia](/connectors/sendgrid/) del conector.

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)