---
title: 'Creación y administración de flujos de trabajo automatizados con Visual Studio Code: Azure Logic Apps | Microsoft Docs'
description: Guía de inicio rápido sobre cómo crear y administrar aplicaciones lógicas con JSON en Visual Studio Code (VS Code)
services: logic-apps
ms.service: logic-apps
ms.workload: azure-vs
author: ecfan
ms.author: estfan
ms.topic: article
ms.reviewer: klam, deli, LADocs
ms.suite: integration
ms.date: 10/05/2018
ms.openlocfilehash: 0fec590523fa130af2e5670a92914c056df289d1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60512248"
---
# <a name="quickstart-create-and-manage-automated-logic-app-workflows---visual-studio-code"></a>Inicio rápido: Creación y administración de flujos de trabajo automatizados de aplicaciones lógicas: Visual Studio Code

Con [Azure Logic Apps](../logic-apps/logic-apps-overview.md) y Visual Studio Code, puede crear y administrar aplicaciones lógicas que le ayuden a automatizar tareas, flujos de trabajo y procesos para integrar aplicaciones, datos, sistemas y servicios entre organizaciones y empresas. En este tutorial rápido se muestra cómo puede crear y editar definiciones de flujo de trabajo de aplicación lógica mediante el esquema de definición de flujo de trabajo en JavaScript Object Notation (JSON) a través de una experiencia basada en código. También puede trabajar en aplicaciones lógicas existentes ya implementadas en <a href="https://docs.microsoft.com/azure/guides/developer/azure-developer-guide" target="_blank">Azure</a> en la nube. 

Aunque puede realizar estas mismas tareas en <a href="https://portal.azure.com" target="_blank">Azure Portal</a> y en Visual Studio, puede empezar a trabajar con mayor rapidez en Visual Studio Code si ya tiene familiaridad con la aplicación lógica y desea trabajar directamente en el código. Por ejemplo, puede deshabilitar, habilitar, eliminar y actualizar las aplicaciones lógicas ya creadas. Además, puede trabajar en Logic Apps y las cuentas de integración desde cualquier plataforma de desarrollo donde se ejecute Visual Studio Code, como Linux, Windows y Mac.

Para este artículo, puede crear la misma aplicación lógica que en la [guía de inicio rápido sobre la creación de una aplicación lógica en Azure Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md), que se centra más en los conceptos básicos. En Visual Studio Code, la aplicación lógica se parece a este ejemplo:

![Aplicación lógica terminada](./media/create-logic-apps-visual-studio-code/overview.png)

Antes de empezar, asegúrese de que dispone de estos elementos:

* Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>.

* Conocimientos básicos acerca de las [definiciones de flujos de trabajo](../logic-apps/logic-apps-workflow-definition-language.md) y su estructura, que usa la notación de objetos JavaScript (JSON) 

  Si no está familiarizado con Logic Apps, pruebe la guía de inicio rápido, que se centra sobre todo en los conceptos básicos y le guía a través del proceso de [creación de su primera aplicación lógica en Azure Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

* Acceso a la web para iniciar sesión en Azure y en su suscripción

* Descargue e instale estas herramientas si no las tiene aún: 

  * <a href="https://code.visualstudio.com/" target="_blank">Visual Studio Code versión 1.25.1 o posterior</a>, que es gratis

  * La extensión de Visual Studio Code para Azure Logic Apps

    Puede descargar e instalar esta extensión desde [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-logicapps) o directamente desde Visual Studio Code. 
    Asegúrese de que se vuelva a cargar Visual Studio Code después de realizar la instalación. 

    ![Buscar "Extensión de Visual Studio Code para Azure Logic Apps"](./media/create-logic-apps-visual-studio-code/find-install-logic-apps-extension.png)

    Para comprobar que la extensión se ha instalado correctamente, el icono de Azure debe aparecer en la barra de herramientas de Visual Studio Code. 

    ![Extensión instalada](./media/create-logic-apps-visual-studio-code/installed-extension.png)

    Para más información, consulte <a href="https://code.visualstudio.com/docs/editor/extension-gallery" target="_blank">Extension Marketplace</a> (Marketplace de extensiones). También puede ver y enviar sus contribuciones a la versión de código abierto de esta extensión; para ello, consulte la [extensión de Azure Logic Apps para Visual Studio Code en GitHub](https://github.com/Microsoft/vscode-azurelogicapps). 

<a name="sign-in-azure"></a>

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

1. Abra Visual Studio Code. En la barra de herramientas de Visual Studio Code, seleccione el icono de Azure. 

   ![Seleccionar icono de Azure](./media/create-logic-apps-visual-studio-code/open-extension.png)

1. En la ventana de Azure, en **Logic Apps**, seleccione **Iniciar sesión en Azure**. 

   ![Seleccionar "Iniciar sesión en Azure"](./media/create-logic-apps-visual-studio-code/sign-in-azure.png)

   Ahora se le pedirá que inicie sesión mediante el código de autenticación proporcionado. 

1. Copie el código de autenticación y, a continuación, elija **Copiar y abrir** y se abrirá una nueva ventana del explorador.

   ![Solicitud de inicio de sesión](./media/create-logic-apps-visual-studio-code/sign-in-prompt.png)

1. Especifique el código de autenticación. Cuando se le solicite, elija **Continuar**.

   ![Especificar código](./media/create-logic-apps-visual-studio-code/authentication-code.png)

1. Seleccione su cuenta de Azure. Después de iniciar sesión, puede cerrar el explorador y volver a Visual Studio Code.

   En la ventana de Azure, en el panel de Logic Apps y en el de cuentas de integración aparecerán ahora las suscripciones de Azure en su cuenta. 

   ![Selección de la suscripción](./media/create-logic-apps-visual-studio-code/select-azure-subscription.png)

   Si no ve las suscripciones que espera, junto a la etiqueta **Logic Apps**, elija **Seleccionar suscripciones** (icono de filtro). Busque y seleccione las suscripciones que desea.

1. Para poder ver cualquier aplicación lógica o cuenta de integración de su suscripción de Azure, expanda la suscripción.

   ![Ver aplicaciones lógicas y cuentas de integración](./media/create-logic-apps-visual-studio-code/existing-logic-apps.png)

<a name="create-logic-app"></a>

## <a name="create-logic-app"></a>Creación de la aplicación lógica

1. Si aún no ha iniciado sesión en su suscripción de Azure desde dentro de Visual Studio Code, siga los pasos descritos en este artículo para [iniciar sesión ahora](#sign-in-azure).

1. En el menú contextual de la suscripción, seleccione **Crear**.

   ![Seleccionar "Crear"](./media/create-logic-apps-visual-studio-code/create-logic-app.png)

1. En la lista que muestra los grupos de recursos de Azure de su suscripción, seleccione un grupo de recursos existente o **cree uno nuevo**. 

   En este ejemplo se crea un grupo de recursos nuevo:

   ![Crear un grupo de recursos](./media/create-logic-apps-visual-studio-code/select-or-create-azure-resource-group.png)

1. Proporcione un nombre para el grupo de recursos de Azure y, a continuación, presione ENTRAR.

   ![Asignar un nombre al grupo de recursos](./media/create-logic-apps-visual-studio-code/enter-name-resource-group.png)

1. Seleccione la ubicación del centro de datos para guardar los metadatos de la aplicación lógica.

   ![Seleccionar ubicación](./media/create-logic-apps-visual-studio-code/select-location.png)

1. Proporcione un nombre para la aplicación lógica y, a continuación, presione ENTRAR.

   ![Asignar un nombre a la aplicación lógica](./media/create-logic-apps-visual-studio-code/enter-name-logic-app.png)

   La nueva aplicación lógica aparece ahora en la ventana de Azure, en su suscripción. Ahora puede empezar a crear la definición del flujo de trabajo de la aplicación lógica.

1. En el menú contextual de la aplicación lógica, seleccione **Abrir en el editor**. 

   ![Abrir aplicación lógica en el editor](./media/create-logic-apps-visual-studio-code/open-new-logic-app.png)

   Visual Studio Code abre una plantilla de definición de flujo de trabajo de aplicación lógica (archivo logicapp.json) para que pueda empezar a crear el flujo de trabajo de la aplicación lógica.

   ![Nueva definición del flujo de trabajo de la aplicación lógica](./media/create-logic-apps-visual-studio-code/blank-logic-app-workflow-definition.png)

1. En el archivo de plantilla de definición del flujo de trabajo de la aplicación lógica, empiece a compilar esta definición. Para más información técnica, consulte el [esquema del lenguaje de definición de flujo de trabajo para Azure Logic Apps](../logic-apps/logic-apps-workflow-definition-language.md).

   Este es un ejemplo de definición lógica. Normalmente, los elementos JSON aparecen ordenados alfabéticamente dentro de cada sección, pero este ejemplo muestra estos elementos aproximadamente en el mismo orden en el que aparecen los pasos de la aplicación lógica en el diseñador.

   ```json
   {
      "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
         "$connections": {
            "defaultValue": {},
            "type": "Object"
         }
      },
      "triggers": {
         "When_a_feed_item_is_published": {
            "recurrence": {
               "frequency": "Minute",
               "interval": 1
            },
            "splitOn": "@triggerBody()?['value']",
            "type": "ApiConnection",
            "inputs": {
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['rss']['connectionId']"
                  }
               },
               "method": "get",
               "path": "/OnNewFeed",
               "queries": {
                  "feedUrl": "http://feeds.reuters.com/reuters/topNews"
               }
            }
         }
      },
      "actions": {
         "Send_an_email": {
            "runAfter": {},
            "type": "ApiConnection",
            "inputs": {
               "body": {
                  "Body": "Title: @{triggerBody()?['title']}\n\nDate published: @{triggerBody()?['publishDate']}\n\nLink: @{triggerBody()?['primaryLink']}",
                  "Subject": "New RSS item: @{triggerBody()?['title']}",
                  "To": "Sophie.Owen@contoso.com"
               },
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['outlook']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/Mail"
            }
         }
      },
      "outputs": {}
   }   
   ```

1. Cuando haya terminado, guarde el archivo de definición de la aplicación lógica. Cuando Visual Studio Code le solicite que confirme la carga de la definición de la aplicación lógica en su suscripción de Azure, elija **Cargar**.

   ![Cargar la nueva aplicación lógica](./media/create-logic-apps-visual-studio-code/upload-new-logic-app.png)

   Una vez que Visual Studio Code publique la aplicación lógica en Azure, puede ver que la aplicación está ya activa y en ejecución en Azure Portal. 

   ![Aplicación lógica publicada en Azure Portal](./media/create-logic-apps-visual-studio-code/published-logic-app.png)

<a name="edit-logic-app"></a>

## <a name="edit-logic-app"></a>Edición de la aplicación lógica

Para trabajar en una aplicación lógica existente que ya esté implementada en Azure, puede abrir el archivo de definición del flujo de trabajo de la aplicación en Visual Studio Code.

1. Si aún no ha iniciado sesión en su suscripción de Azure desde dentro de Visual Studio Code, siga los pasos descritos en este artículo para [iniciar sesión ahora](#sign-in-azure).

1. En la ventana de Azure, en **Logic Apps**, expanda su suscripción de Azure y seleccione la aplicación lógica que quiera. 

1. En el menú de la aplicación lógica, seleccione **Abrir en el editor**. O bien, junto al nombre de la aplicación lógica, elija el icono de edición.

   ![Abrir el editor para la aplicación lógica existente](./media/create-logic-apps-visual-studio-code/open-editor-existing-logic-app.png)

   Visual Studio Code abre el archivo logicapp.json para la definición del flujo de trabajo de la aplicación lógica.

   ![Definición abierta del flujo de trabajo de la aplicación lógica](./media/create-logic-apps-visual-studio-code/edit-logic-app-workflow-definition-file.png)

1. Realice los cambios en la definición de la aplicación lógica.

1. Cuando haya terminado, guarde los cambios.

1. Cuando Visual Studio Code le solicite que actualice la carga de la definición de la aplicación lógica en su suscripción de Azure, elija **Cargar**. 

   ![Cargar sus ediciones](./media/create-logic-apps-visual-studio-code/upload-logic-app-changes.png)

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">foro de Azure Logic Apps</a>.
* Para enviar ideas sobre características o votar sobre ellas, visite el <a href="https://aka.ms/logicapps-wish" target="_blank">sitio de comentarios de los usuarios de Logic Apps</a>.

