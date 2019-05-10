---
title: Cree aplicaciones sin servidor con Azure Logic Apps y Azure Functions en Visual Studio
description: Cree, implemente y administre su primera aplicación sin servidor con Azure Logic Apps y Azure Functions en Visual Studio
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.custom: vs-azure
ms.topic: article
ms.date: 04/25/2019
ms.openlocfilehash: fb071ac056355a056e5333284808417a258515de
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65467448"
---
# <a name="build-your-first-serverless-app-with-azure-logic-apps-and-azure-functions---visual-studio"></a>Cree su primera aplicación sin servidor con Azure Logic Apps y Azure Functions (Visual Studio)

Con las herramientas sin servidor y funcionalidades de Azure, como [Azure Logic Apps](../logic-apps/logic-apps-overview.md) y [Azure Functions](../azure-functions/functions-overview.md) se pueden desarrollar e implementar rápidamente aplicaciones en la nube. En este artículo se muestra cómo empezar a crear una aplicación sin servidor, que usa una aplicación lógica que llama a una función de Azure, en Visual Studio. Para más información acerca de las soluciones sin servidor en Azure, consulte [Introducción a Azure sin servidor con Functions y Logic Apps](../logic-apps/logic-apps-serverless-overview.md).

## <a name="prerequisites"></a>Requisitos previos

Para crear una aplicación sin servidor en Visual Studio se necesitan estos elementos:

* Una suscripción de Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Descargue e instale estas herramientas si no las tiene aún:

  * [Visual Studio 2019, 2017 o 2015 Community Edition o superior](https://aka.ms/download-visual-studio). 
  Este inicio rápido usa Visual Studio Community 2017, que es gratuito.

    > [!IMPORTANT]
    > Al instalar Visual Studio de 2019 o 2017, asegúrese de que selecciona la carga de trabajo **desarrollo de Azure**.
    > Para Visual Studio 2019, Cloud Explorer puede abrir el Diseñador de aplicación lógica en Azure Portal, pero aún no se puede abrir el Diseñador de aplicación lógica insertado.

  * [Microsoft Azure SDK para .NET (2.9.1 o una versión posterior)](https://azure.microsoft.com/downloads/). 
  Más información sobre [Azure SDK for .NET](https://docs.microsoft.com/dotnet/azure/dotnet-tools?view=azure-dotnet).

  * [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

  * Herramientas de Azure Logic Apps para la versión de Visual Studio que desee:

    * [Visual Studio 2019](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2019)

    * [Visual Studio 2017](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2017)

    * [Visual Studio 2015](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2015)
  
    Puede descargar e instalar Herramientas de Azure Logic Apps directamente desde Visual Studio Marketplace o aprender [a instalar esta extensión desde Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions). 
    Asegúrese de reiniciar Visual Studio después de finalizar la instalación.

  * [Herramientas principales de Azure Functions](https://www.npmjs.com/package/azure-functions-core-tools) para depurar Functions localmente

* Acceso a la web al usar el Diseñador de aplicaciones lógicas integrado

  El diseñador requiere una conexión a Internet para crear recursos en Azure y leer las propiedades y los datos de los conectores de la aplicación lógica. 
  Por ejemplo, si usa el conector de Dynamics CRM Online, el diseñador comprueba en la instancia de CRM si hay propiedades predeterminadas y propiedades personalizadas disponibles.

## <a name="create-resource-group-project"></a>Creación de un proyecto de grupo de recursos

Para empezar, cree un [proyecto de grupo de recursos de Azure](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) para su aplicación sin servidor. En Azure, los recursos se crean dentro de un grupo de recursos, que es una colección lógica que se usa para organizar, administrar e implementar recursos en toda una aplicación como un solo recurso. Para una aplicación sin servidor de Azure, el grupo de recursos incluye recursos para Azure Logic Apps y Azure Functions. Más información sobre [grupos de recursos y recursos de Azure](../azure-resource-manager/resource-group-overview.md).

1. Inicie Visual Studio e inicie sesión con su cuenta de Azure.

1. En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.

   ![Creación de un proyecto en Visual Studio](./media/logic-apps-serverless-get-started-vs/create-new-project-visual-studio.png)

1. En **Instalado**, seleccione **Visual C#** o **Visual Basic**. Seleccione **Nube** > **Grupo de recursos de Azure**.

   > [!NOTE]
   > Si la categoría **Nube** o el proyecto **Grupo de recursos de Azure** no existen, asegúrese de que ha instalado el SDK de Azure para Visual Studio.

   Si usa Visual Studio 2019, siga estos pasos:

   1. En el cuadro **Crear un proyecto**, seleccione la plantilla de proyectos **Grupo de recursos de Azure** para Visual C# o Visual Basic, y elija **Siguiente**.

   1. Proporcione el nombre del grupo de recursos de Azure que desea usar y otra información adicional sobre el proyecto. Cuando termine, seleccione **Crear**.

1. Asigne un nombre y una ubicación al proyecto y elija **Aceptar**.

   Visual Studio le pedirá que seleccione una plantilla de la lista de plantillas. 
   En este ejemplo usa una plantilla de inicio rápido de Azure para que pueda crear una aplicación sin servidor que incluye una aplicación lógica y una llamada a una función de Azure.

   > [!TIP]
   > En escenarios donde no desea predeploy su solución en un grupo de recursos de Azure, puede usar el espacio en blanco **aplicación lógica** plantilla, que simplemente crea una aplicación lógica vacía.

1. Desde el **mostrar las plantillas desde esta ubicación** lista, seleccione **inicio rápido de Azure (github.com/Azure/azure-quickstart-templates)**.

1. En el cuadro de búsqueda, escriba "aplicación lógica" como filtro. En los resultados, seleccione esta plantilla: **101-logic-app-and-function-app**

   ![Seleccionar una plantilla de inicio rápido de Azure](./media/logic-apps-serverless-get-started-vs/select-template.png)

   Visual Studio crea y abre una solución para el proyecto del grupo de recursos. 
   La plantilla de inicio rápido de Azure seleccionada crea una plantilla de implementación denominada `azuredeploy.json` dentro de su proyecto de grupo de recursos. Dicha plantilla incluye la definición de una aplicación lógica sencilla que se desencadena en una solicitud HTTP, llama a una función de Azure y devuelve el resultado en forma de respuesta HTTP.

   ![Nueva solución sin servidor](./media/logic-apps-serverless-get-started-vs/create-serverless-solution.png)

1. A continuación, debe implementar la solución en Azure para poder abrir la plantilla de implementación y revisar los recursos de la aplicación sin servidor.

## <a name="deploy-your-solution"></a>Implementación de la solución

Para poder abrir la aplicación lógica con el Diseñador de aplicaciones lógicas en Visual Studio, debe tener un grupo de recursos de Azure que ya esté implementado en Azure. A partir de ahí, el diseñador puede crear conexiones a los recursos y servicios de la aplicación lógica. Para esta tarea, implemente la solución de Visual Studio en Azure Portal.

1. En el Explorador de soluciones, desde el menú contextual de su proyecto recursos, seleccione **implementar** > **New**.

   ![Crear una implementación nueva para el grupo de recursos](./media/logic-apps-serverless-get-started-vs/deploy.png)

1. Si no está seleccionada, seleccione su suscripción de Azure y el grupo de recursos en el que desee realizar la implementación. Elija **Implementar**.

   ![Configuración de la implementación](./media/logic-apps-serverless-get-started-vs/deploy-to-resource-group.png)

1. Si aparece el cuadro **Editar parámetros**, especifique el nombre de recurso que va a usar para la aplicación lógica y la aplicación de función de Azure en la implementación y guarde la configuración. Asegúrese de que usa un nombre globalmente único para la aplicación de función.

   ![Especificar nombres para la aplicación lógica y la aplicación de función](./media/logic-apps-serverless-get-started-vs/logic-function-app-name-parameters.png)

   Cuando Visual Studio inicia la implementación en el grupo de recursos especificado, el estado de implementación de la solución aparece en la ventana **Salida** de Visual Studio. 
   Una vez finalizada la implementación, la aplicación lógica está activa en Azure Portal.

## <a name="edit-logic-app-in-visual-studio"></a>Edición de una aplicación lógica en Visual Studio

Ahora que la solución se implementa en el grupo de recursos, abra la aplicación lógica con el Diseñador de aplicación lógica para que pueda editar y cambiar la aplicación lógica.

1. En el Explorador de soluciones, desde el `azuredeploy.json` menú contextual del archivo, seleccione **abrir con Diseñador de aplicaciones lógicas**.

   ![Abra "azuredeploy.json" en el Diseñador de aplicación lógica](./media/logic-apps-serverless-get-started-vs/open-logic-app-designer.png)

1. Después de que el cuadro **Propiedades de la aplicación lógica** aparezca, y si no está seleccionado, en **Suscripción**, seleccione su suscripción de Azure. En **Grupo de recursos**, seleccione el grupo de recursos y la ubicación en que ha implementado la solución y elija **Aceptar**.

   ![Propiedades de la aplicación lógica](./media/logic-apps-serverless-get-started-vs/logic-app-properties.png)

   Cuando se abra el Diseñador de aplicaciones lógicas, puede agregar más pasos o cambiar el flujo de trabajo y guardar las actualizaciones.

   ![Aplicación lógica abierta en el Diseñador de aplicación lógica](./media/logic-apps-serverless-get-started-vs/opened-logic-app.png)

## <a name="create-azure-functions-project"></a>Creación de un proyecto de Azure Functions

Para crear el proyecto de Functions y una función con JavaScript, Python, F#, PowerShell, Batch o Bash, siga los pasos descritos en el artículo [Uso de Azure Functions Core Tools](../azure-functions/functions-run-local.md). Para desarrollar una función de Azure con C# en la solución, puede usar una biblioteca de clases de C#, para lo que debe seguir los pasos descritos en el artículo [Publish a .NET class library as a Function App](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/) (Publicar una biblioteca de clases .NET como una instancia de Function App).

## <a name="deploy-functions-from-visual-studio"></a>Implementación de funciones desde Visual Studio

La plantilla de implementación implementa las funciones de Azure que tiene en su solución desde el repositorio de Git que especifiquen las variables en el archivo `azuredeploy.json`. Si crea un proyecto de Functions en su solución, puede marcarlo en el control de código fuente de Git, por ejemplo, GitHub o Azure DevOps, y luego actualizar la variable `repo` para que la plantilla implemente la función de Azure.

## <a name="manage-logic-apps-and-view-run-history"></a>Administración de aplicaciones lógicas y visualización del historial de ejecuciones

En el caso de las aplicaciones lógicas ya implementadas en Azure, puede editarlas, administrarlas, ver su historial de ejecución y deshabilitarlas desde Visual Studio.

1. En el menú **Ver** de Visual Studio, abra **Cloud Explorer**.

1. En **Todas las suscripciones**, seleccione la suscripción de Azure asociada a las aplicaciones de lógica que desea administrar y elija **Aplicar**.

1. En **Logic Apps**, seleccione la aplicación lógica. En el menú contextual de la aplicación, seleccione **Abrir con el editor de aplicaciones lógicas**.

Ya puede descargar la aplicación lógica que ha publicado en el proyecto del grupo de recursos. Por tanto, aunque pueda haber iniciado una aplicación lógica en Azure Portal, puede importarla y administrarla en Visual Studio. Para más información, consulte [Administración de aplicaciones lógicas con Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md).

## <a name="next-steps"></a>Pasos siguientes

* [Administración de aplicaciones lógicas con Visual Studio](manage-logic-apps-with-visual-studio.md)
