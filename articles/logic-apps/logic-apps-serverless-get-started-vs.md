---
title: Compilación de aplicaciones sin servidor con Visual Studio | Microsoft Docs
description: Cree, implemente y administre su primera aplicación sin servidor con Azure Logic Apps y Azure Functions en Visual Studio
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.reviewer: jehollan, LADocs
ms.suite: integration
ms.custom: vs-azure
ms.topic: article
ms.date: 08/01/2018
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.openlocfilehash: ed56e5f2a4a0d9e3a5e8d8c80e0fd20dbfb098bb
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444793"
---
# <a name="build-your-first-serverless-app-with-azure-logic-apps-and-azure-functions---visual-studio"></a>Cree su primera aplicación sin servidor con Azure Logic Apps y Azure Functions (Visual Studio)

Con las herramientas sin servidor y funcionalidades de Azure, como [Azure Logic Apps](../logic-apps/logic-apps-overview.md) y [Azure Functions](../azure-functions/functions-overview.md) se pueden desarrollar e implementar rápidamente aplicaciones en la nube. En este artículo se muestra cómo empezar a crear una aplicación sin servidor, que usa una aplicación lógica que llama a una función de Azure, en Visual Studio. Para más información acerca de las soluciones sin servidor en Azure, consulte [Introducción a Azure sin servidor con Functions y Logic Apps](../logic-apps/logic-apps-serverless-overview.md).

## <a name="prerequisites"></a>Requisitos previos

Para crear una aplicación sin servidor en Visual Studio se necesitan estos elementos:

* Una suscripción de Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* [Visual Studio 2017](https://www.visualstudio.com/vs/) o Visual Studio 2015 - Community, Professional o Enterprise

* [Microsoft Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 o posterior)

* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

* [Herramientas de Azure Logic Apps para Visual Studio de 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) o la [versión de Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio)

  Herramientas de Azure Logic Apps se puede descargar e instalar directamente desde Visual Studio Marketplace, o bien puede [aprender a instalar esta extensión desde el propio Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions). 
  Asegúrese de reiniciar Visual Studio después de finalizar la instalación. 

* [Herramientas principales de Azure Functions](https://www.npmjs.com/package/azure-functions-core-tools) para depurar Functions localmente

* Acceso a la web mientras usa el Diseñador de aplicación lógica insertado en Visual Studio

  El diseñador requiere una conexión a Internet para crear recursos en Azure y leer las propiedades y los datos de los conectores de la aplicación lógica. 
  Por ejemplo, si usa el conector de Dynamics CRM Online, el diseñador comprueba en la instancia de CRM si hay propiedades predeterminadas y propiedades personalizadas disponibles.

## <a name="create-resource-group-project"></a>Creación de un proyecto de grupo de recursos

Para empezar, cree un [proyecto de grupo de recursos de Azure](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) para su aplicación sin servidor. En Azure, los recursos se crean dentro de un grupo de recursos, que es una colección lógica que se usa para organizar, administrar e implementar recursos en toda una aplicación como un solo recurso. Para una aplicación sin servidor de Azure, el grupo de recursos incluye recursos para Azure Logic Apps y Azure Functions. Más información sobre [grupos de recursos y recursos de Azure](../azure-resource-manager/resource-group-overview.md).

1. Inicie Visual Studio e inicie sesión con su cuenta de Azure. 

1. En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**. 

   ![Creación de un proyecto en Visual Studio](./media/logic-apps-serverless-get-started-vs/create-new-project-visual-studio.png)

1. En **Instalado**, seleccione **Visual C#** o **Visual Basic**. Seleccione **Nube** > **Grupo de recursos de Azure**.

   Si la categoría **Nube** o el proyecto **Grupo de recursos de Azure** no existen, asegúrese de que ha instalado el SDK de Azure para Visual Studio.

1. Asigne un nombre y una ubicación al proyecto y elija **Aceptar**. 

   Visual Studio le pedirá que seleccione una plantilla. 
   Puede comenzar con una en blanco, Aplicación lógica u otra plantilla, pero en este ejemplo se usa una plantilla de inicio rápido de Azure para la creación de una aplicación sin servidor que incluye una aplicación lógica y una llamada a una función de Azure.

   Para crear solo una aplicación lógica en Visual Studio, seleccione la plantilla de **Aplicación lógica**. Esta plantilla crea una aplicación lógica vacía que se abre en el Diseñador de aplicaciones lógicas sin que sea preciso implementarla previamente en un grupo de recursos de Azure.

1. En **Show templates from this location** (Mostrar las plantillas de esta ubicación), seleccione **Inicio rápido de Azure (github/Azure/azure-quickstart-templates)**. 

1. En el cuadro de búsqueda, escriba "aplicación lógica" como filtro, seleccione esta plantilla de inicio rápido sin servidor y elija **Aceptar**: **101-logic-app-and-function-app**

   ![Seleccionar una plantilla de inicio rápido de Azure](./media/logic-apps-serverless-get-started-vs/select-template.png)

   Visual Studio crea y abre una solución para el proyecto del grupo de recursos. 
   La plantilla de inicio rápido que ha seleccionado crea una plantilla de implementación llamada `azuredeploy.json` en el proyecto del grupo de recursos. 
   Dicha plantilla incluye la definición de una aplicación lógica sencilla que se desencadena en una solicitud HTTP, llama a una función de Azure y devuelve el resultado en forma de respuesta HTTP. 
   
   ![Nueva solución sin servidor](./media/logic-apps-serverless-get-started-vs/create-serverless-solution.png)

1. A continuación, debe implementar la solución en Azure para poder abrir la plantilla de implementación y revisar los recursos de la aplicación sin servidor. 

## <a name="deploy-your-solution"></a>Implementación de la solución

Para poder abrir la aplicación lógica con el Diseñador de aplicaciones lógicas en Visual Studio, debe tener un grupo de recursos de Azure que ya esté implementado en Azure. A partir de ahí, el diseñador puede crear conexiones a los recursos y servicios de la aplicación lógica. Para esta tarea, implemente la solución de Visual Studio en Azure Portal.

1. En el Explorador de soluciones, abra el menú contextual del proyecto y seleccione **Implementar** > **Nuevo**.

   ![Crear una implementación nueva para el grupo de recursos](./media/logic-apps-serverless-get-started-vs/deploy.png)

1. Si no está seleccionada, seleccione su suscripción de Azure y el grupo de recursos en el que desee realizar la implementación. Elija **Implementar**.

   ![Configuración de implementación](./media/logic-apps-serverless-get-started-vs/deploy-to-resource-group.png)

1. Si aparece el cuadro **Editar parámetros**, especifique el nombre de recurso que va a usar para la aplicación lógica y la aplicación de función de Azure en la implementación y guarde la configuración. Asegúrese de que usa un nombre globalmente único para la aplicación de función.

   ![Especificar nombres para la aplicación lógica y la aplicación de función](./media/logic-apps-serverless-get-started-vs/logic-function-app-name-parameters.png)

   Cuando Visual Studio inicia la implementación en el grupo de recursos especificado, el estado de implementación de la solución aparece en la ventana **Salida** de Visual Studio. 
   Una vez finalizada la implementación, la aplicación lógica está activa en Azure Portal.

## <a name="edit-logic-app-in-visual-studio"></a>Edición de una aplicación lógica en Visual Studio

Ahora que la solución está implementada en el grupo de recursos, abra la aplicación lógica con el Diseñador de aplicación lógica para poder editarla y cambiarla.

1. En el Explorador de soluciones, abra el menú contextual del archivo `azuredeploy.json` y, después, seleccione **Abrir con el Diseñador de aplicación lógica**.

   ![Abra "azuredeploy.json" en el Diseñador de aplicación lógica](./media/logic-apps-serverless-get-started-vs/open-logic-app-designer.png)

1. Después de que el cuadro **Propiedades de la aplicación lógica** aparezca, y si no está seleccionado, en **Suscripción**, seleccione su suscripción de Azure. En **Grupo de recursos**, seleccione el grupo de recursos y la ubicación en que ha implementado la solución y elija **Aceptar**.

   ![Propiedades de la aplicación lógica](./media/logic-apps-serverless-get-started-vs/logic-app-properties.png)

   Cuando se abra el Diseñador de aplicaciones lógicas, puede agregar más pasos o cambiar el flujo de trabajo y guardar las actualizaciones.

   ![Aplicación lógica abierta en el Diseñador de aplicación lógica](./media/logic-apps-serverless-get-started-vs/opened-logic-app.png)

## <a name="create-azure-functions-project"></a>Creación de un proyecto de Azure Functions

Para crear el proyecto de Functions y una función con JavaScript, Python, F#, PowerShell, Batch o Bash, siga los pasos descritos en el artículo [Uso de Azure Functions Core Tools](../azure-functions/functions-run-local.md). Para desarrollar una función de Azure con C# en la solución, puede usar una biblioteca de clases de C#, para lo que debe seguir los pasos descritos en el artículo [Publish a .NET class library as a Function App](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/) (Publicar una biblioteca de clases .NET como una instancia de Function App).

## <a name="deploy-functions-from-visual-studio"></a>Implementación de funciones desde Visual Studio

La plantilla de implementación implementa las funciones de Azure que tiene en su solución desde el repositorio de Git que especifiquen las variables en el archivo `azuredeploy.json`. Si crea un proyecto de Functions en su solución, puede marcarlo en el control de código fuente de Git, por ejemplo, GitHub o Visual Studio Team Services y, después, actualizar la variable `repo` para que la plantilla implemente la función de Azure.

## <a name="manage-logic-apps-and-view-run-history"></a>Administración de aplicaciones lógicas y visualización del historial de ejecuciones

En el caso de las aplicaciones lógicas ya implementadas en Azure, puede editarlas, administrarlas, ver su historial de ejecución y deshabilitarlas desde Visual Studio. 

1. En el menú **Ver** de Visual Studio, abra **Cloud Explorer**. 

1. En **Todas las suscripciones**, seleccione la suscripción de Azure asociada a las aplicaciones de lógica que desea administrar y elija **Aplicar**.

1. En **Logic Apps**, seleccione la aplicación lógica. En el menú contextual de la aplicación, seleccione **Abrir con el editor de aplicaciones lógicas**. 

Ya puede descargar la aplicación lógica que ha publicado en el proyecto del grupo de recursos. Por tanto, aunque pueda haber iniciado una aplicación lógica en Azure Portal, puede importarla y administrarla en Visual Studio. Para más información, consulte [Administración de aplicaciones lógicas con Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md).

## <a name="next-steps"></a>Pasos siguientes

* [Creación de un panel social sin servidor](logic-apps-scenario-social-serverless.md)
* [Administración de aplicaciones lógicas con Visual Studio](manage-logic-apps-with-visual-studio.md)
* [Lenguaje de definición de flujo de trabajo en una aplicación lógica](logic-apps-workflow-definition-language.md)