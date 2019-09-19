---
title: Administración de aplicaciones lógicas mediante Visual Studio - Azure Logic Apps
description: Administración de aplicaciones lógicas y otros recursos de Azure con Visual Studio Cloud Explorer
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.custom: mvc
ms.date: 05/07/2019
ms.openlocfilehash: dd6cd16302c69266a954816868c04c8507762717
ms.sourcegitcommit: b7b0d9f25418b78e1ae562c525e7d7412fcc7ba0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/08/2019
ms.locfileid: "70801264"
---
# <a name="manage-logic-apps-with-visual-studio"></a>Administración de aplicaciones lógicas con Visual Studio

Aunque puede crear, editar, administrar e implementar aplicaciones lógicas en [Azure Portal](https://portal.azure.com), también puede usar Visual Studio cuando quiera agregar las aplicaciones lógicas al control de código fuente, publicar versiones diferentes y crear plantillas de [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) para distintos entornos de implementación. Con Visual Studio Cloud Explorer, puede encontrar y administrar las aplicaciones lógicas junto con otros recursos de Azure. Por ejemplo, puede abrir, descargar, editar, ejecutar, ver el historial de ejecución, deshabilitar y habilitar las aplicaciones lógicas que ya están implementadas en Azure Portal. Si es la primera vez que usa Azure Logic Apps en Visual Studio, obtenga información sobre [cómo crear aplicaciones lógicas con Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

> [!IMPORTANT]
> Al implementar o publicar una aplicación lógica desde Visual Studio se sobrescribe la versión de esa aplicación en Azure Portal. Por tanto, si hace cambios en Azure Portal que desea mantener, asegúrese de [actualizar la aplicación lógica en Visual Studio](#refresh) desde Azure Portal antes de volver a implementar o publicar desde Visual Studio.

<a name="requirements"></a>

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Descargue e instale estas herramientas si no las tiene aún: 

  * [Visual Studio 2019, 2017 o 2015 Community Edition o superior](https://aka.ms/download-visual-studio). 
  Este inicio rápido usa Visual Studio Community 2017, que es gratuito.

    > [!IMPORTANT]
    > Al instalar Visual Studio de 2019 o 2017, asegúrese de que selecciona la carga de trabajo **desarrollo de Azure**.
    > Para obtener más información, vea [Administración de los recursos asociados con las cuentas de Azure en Visual Studio Cloud Explorer](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-resources-managing-with-cloud-explorer?view).

    Para instalar Cloud Explorer para Visual Studio 2015, [descargue Cloud Explorer desde Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015). 
    Para obtener más información, vea [Administración de los recursos asociados con las cuentas de Azure en Visual Studio Cloud Explorer (2015)](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-resources-managing-with-cloud-explorer?view=vs-2015).

  * [SDK de Azure (2.9.1 o posterior)](https://azure.microsoft.com/downloads/) 

  * [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

  * Herramientas de Azure Logic Apps para la versión de Visual Studio que desee:

    * [Visual Studio 2019](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2019)

    * [Visual Studio 2017](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2017)

    * [Visual Studio 2015](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2015)

    Puede descargar e instalar Herramientas de Azure Logic Apps directamente desde Visual Studio Marketplace o aprender [a instalar esta extensión desde Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions). 
    Asegúrese de reiniciar Visual Studio después de finalizar la instalación.

* Acceso a la Web mediante el diseñador integrado de Logic Apps

  El diseñador requiere una conexión a Internet para crear recursos en Azure y leer las propiedades y los datos de los conectores de la aplicación lógica. 
  Por ejemplo, si usa el conector de Dynamics CRM Online, el diseñador comprueba en la instancia de CRM si hay propiedades predeterminadas y propiedades personalizadas disponibles.

<a name="find-logic-apps-vs"></a>

## <a name="find-your-logic-apps"></a>Búsqueda de las aplicaciones lógicas

En Visual Studio, puede encontrar todas las aplicaciones lógicas que están asociadas con la suscripción de Azure y que están implementadas en Azure Portal mediante Cloud Explorer.

1. Abra Visual Studio. En el menú **Ver**, seleccione **Cloud Explorer**.

1. En Cloud Explorer, elija **Administración de cuentas**. Seleccione la suscripción de Azure asociada con las aplicaciones lógicas y, luego, elija **Aplicar**. Por ejemplo:

   ![Elección de "Administración de cuentas"](./media/manage-logic-apps-with-visual-studio/account-management-select-Azure-subscription.png)

1. En función de si busca por **Grupos de recursos** o **Tipos de recursos**, siga estos pasos:

   * **Grupos de recursos**: en la suscripción de Azure, Cloud Explorer muestra todos los grupos de recursos que están asociados con esa suscripción. 
   Expanda el grupo de recursos que contiene la aplicación lógica y selecciónela.

   * **Tipos de recursos**: en la suscripción de Azure, expanda **Logic Apps**. Una vez que Cloud Explorer muestra todas las aplicaciones lógicas implementadas que están asociadas a la suscripción, seleccione su aplicación lógica.

<a name="open-designer"></a>

## <a name="open-in-visual-studio"></a>Apertura en Visual Studio

En Visual Studio, puede abrir las aplicaciones lógicas anteriormente creadas e implementadas de manera directa mediante Azure Portal o como proyectos de grupo de recursos de Azure con Visual Studio.

1. Abra Cloud Explorer y busque su aplicación lógica. 

1. En el menú contextual de la aplicación lógica, seleccione **Abrir con el editor Logic App**.

   > [!TIP]
   > Si no tiene este comando en Visual Studio 2019, compruebe que tiene las actualizaciones más recientes de Visual Studio.

   Este ejemplo muestra las aplicaciones lógicas según el tipo de recurso, por lo que las aplicaciones lógicas aparecen en la sección **Logic Apps**.

   ![Abrir aplicación lógica implementada desde Azure Portal](./media/manage-logic-apps-with-visual-studio/open-logic-app-in-editor.png)

   Una vez que la aplicación lógica se abre en el diseñador de Logic Apps, en la parte inferior del diseñador, puede elegir la **Vista de código** para poder revisar la estructura de definición de aplicación lógica subyacente. 
   Si quiere crear una plantilla de implementación para la aplicación lógica, obtenga información sobre [cómo descargar una plantilla de Azure Resource Manager](#download-logic-app) para esa aplicación lógica. Obtenga más información sobre las [plantillas de Resource Manager](../azure-resource-manager/template-deployment-overview.md).

<a name="download-logic-app"></a>

## <a name="download-from-azure"></a>Descarga de Azure

Puede descargar aplicaciones lógicas desde [Azure Portal](https://portal.azure.com) y guardarlas como plantillas de [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). Luego puede editar de manera local las plantillas con Visual Studio y personalizar las aplicaciones lógicas para distintos entornos de implementación. Al descargar las aplicaciones lógicas, se *parametriza* automáticamente sus definiciones dentro de las [plantillas de Resource Manager](../azure-resource-manager/template-deployment-overview.md), que también usan la notación de objetos JavaScript (JSON).

1. En Visual Studio, abra Cloud Explorer y busque y seleccione la aplicación lógica que desea descargar de Azure.

2. En el menú contextual de la aplicación, seleccione **Abrir con el editor de Logic App**.

   > [!TIP]
   > Si no tiene este comando en Visual Studio 2019, compruebe que tiene las actualizaciones más recientes de Visual Studio.

   El Diseñador de aplicaciones lógicas se abre y muestra la aplicación lógica. 
   Para revisar la estructura y la definición subyacentes de aplicaciones lógicas, en la parte inferior del diseñador, elija la **vista Código**. 

3. En la barra de herramientas del diseñador, elija **Descargar**.

   ![Elegir "Descargar"](./media/manage-logic-apps-with-visual-studio/download-logic-app.png)

4. Cuando se le pida una ubicación, vaya a esa ubicación y guarde la plantilla de Resource Manager para la definición de aplicaciones lógicas en formato de archivo JSON (.json). 

La definición de aplicaciones lógicas aparece en la subsección `resources` de la plantilla de Resource Manager. Ahora puede editar la definición de aplicaciones lógicas y la plantilla de Resource Manager con Visual Studio. También puede agregar la plantilla como un [proyecto de grupo de recursos de Azure](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) a una solución de Visual Studio. Aprenda sobre los [proyectos de grupo de recursos de Azure para aplicaciones lógicas en Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md). 

<a name="link-integration-account"></a>

## <a name="link-to-integration-account"></a>Vínculo a la cuenta de integración

Para crear aplicaciones lógicas en escenarios de integración empresarial de negocio a negocio (B2B), puede vincular la aplicación lógica a una [cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) creada anteriormente que exista en la misma región que la aplicación lógica. Una cuenta de integración contiene artefactos B2B, como entidades, acuerdos, esquemas y asignaciones, y permite que la aplicación lógica use conectores B2B para la validación de XML y la codificación o descodificación de archivos sin formato. Aunque puede [crear este vínculo mediante Azure Portal](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account), también puede usar Visual Studio después de cumplir los [requisitos previos](#requirements), y la aplicación lógica existirá como un archivo JSON (.json) dentro de un [proyecto de grupo de recursos de Azure](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). Aprenda sobre los [proyectos de grupo de recursos de Azure para aplicaciones lógicas en Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#create-resource-group-project).

1. En Visual Studio, abra el proyecto de grupo de recursos de Azure que contiene la aplicación lógica.

1. En el Explorador de soluciones, abra el menú contextual del archivo **<nombre de la aplicación lógica>.json** y seleccione **Open With Logic App Designer** (Abrir con el Diseñador de aplicación lógica). (Teclado: Ctrl+L)

   ![Abrir el archivo .json de la aplicación lógica con el Diseñador de aplicación lógica](./media/manage-logic-apps-with-visual-studio/open-logic-app-designer.png)

   > [!TIP]
   > Si no tiene este comando en Visual Studio 2019, compruebe que tiene las actualizaciones más recientes de Visual Studio.

1. Para asegurarse de que el Diseñador de aplicación lógica tiene el foco, seleccione la pestaña o la superficie del diseñador para que el panel Propiedades muestre la propiedad **Cuenta de integración** de la aplicación lógica.

   ![Panel Propiedades que muestra la propiedad "Cuenta de integración"](./media/manage-logic-apps-with-visual-studio/open-logic-app-properties.png)

1. Abra la lista **Cuenta de integración** y seleccione la cuenta de integración que quiere vincular a la aplicación lógica, por ejemplo:

   ![Abrir la lista de propiedades "Cuenta de integración"](./media/manage-logic-apps-with-visual-studio/select-integration-account.png)

1. Cuando haya terminado, no olvide guardar la solución de Visual Studio.

Cuando se establece la propiedad **Cuenta de integración** en Visual Studio y se guarda la aplicación lógica como una plantilla de Azure Resource Manager, esa plantilla también incluye una declaración de parámetros para la cuenta de integración seleccionada. Para más información sobre los parámetros de plantilla y las aplicaciones lógicas, consulte [Introducción: Automatización de la implementación de aplicaciones lógicas](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#template-parameters)

<a name="refresh"></a>

## <a name="refresh-from-azure"></a>Actualización desde Azure

Si edita la aplicación lógica en Azure Portal y quiere mantener esos cambios, asegúrese de actualizar la versión de esa aplicación en Visual Studio con esos cambios. 

* En Visual Studio, en la barra de herramientas del Diseñador de aplicaciones lógicas, elija **Actualizar**.

  O bien

* En Visual Studio Cloud Explorer, abra el menú contextual de la aplicación lógica y seleccione **Actualizar**.

![Actualización de aplicación lógica con actualizaciones](./media/manage-logic-apps-with-visual-studio/refresh-logic-app.png)

## <a name="publish-logic-app-updates"></a>Publicación de actualizaciones de la aplicación lógica

Cuando esté listo para implementar las actualizaciones de la aplicación lógica desde Visual Studio a Azure, en la barra de herramientas del Diseñador de aplicaciones lógicas, elija **Publicar**.

![Publicar aplicación lógica actualizada](./media/manage-logic-apps-with-visual-studio/publish-logic-app.png)

## <a name="manually-run-your-logic-app"></a>Ejecución manual de la aplicación lógica

Puede desencadenar manualmente una aplicación lógica implementada en Azure desde Visual Studio. En la barra de herramientas del Diseñador de aplicaciones lógicas, elija **Ejecutar desencadenador**.

![Ejecución manual de la aplicación lógica](./media/manage-logic-apps-with-visual-studio/manually-run-logic-app.png)

## <a name="review-run-history"></a>Revisar el historial de ejecución.

Para comprobar el estado y diagnosticar problemas con las ejecuciones de la aplicación lógica, puede revisar los detalles, como las entradas y las salidas, para esas ejecuciones en Visual Studio.

1. En Cloud Explorer, abra el menú contextual de la aplicación lógica y seleccione **Abrir el historial de ejecución**.

   ![Abrir el historial de ejecución](./media/manage-logic-apps-with-visual-studio/view-run-history.png)

1. Para ver los detalles de una ejecución específica, haga doble clic en ella. Por ejemplo:

   ![Historial de ejecución detallado](./media/manage-logic-apps-with-visual-studio/view-run-history-details.png)
  
   > [!TIP]
   > Para ordenar la tabla por propiedad, elija el encabezado de columna de esa propiedad. 

1. Expanda los pasos cuyas entradas y salidas desea revisar. Por ejemplo:

   ![Ver las entradas y salidas de cada paso](./media/manage-logic-apps-with-visual-studio/run-inputs-outputs.png)

## <a name="disable-or-enable-logic-app"></a>Deshabilitación o habilitación de la aplicación lógica

Sin eliminar la aplicación lógica, puede evitar que el desencadenador se active la próxima vez que se cumpla una condición desencadenadora. Si deshabilita la aplicación lógica, evita que el motor de Logic Apps cree y ejecute futuras instancias de flujo de trabajo para la aplicación lógica.
En Cloud Explorer, abra el menú contextual de la aplicación lógica y seleccione **Deshabilitar**.

![Deshabilitar la aplicación lógica](./media/manage-logic-apps-with-visual-studio/disable-logic-app.png)

> [!NOTE]
> Cuando se deshabilita una aplicación lógica, no se crean instancias de nuevas ejecuciones. Todas las ejecuciones en curso y pendientes continuarán hasta que finalicen, lo que puede tardar un tiempo en realizarse. 

Cuando esté listo para que la aplicación lógica reanude la operación, puede reactivar la aplicación lógica. En Cloud Explorer, abra el menú contextual de la aplicación lógica y seleccione **Habilitar**.

![Habilitar la aplicación lógica](./media/manage-logic-apps-with-visual-studio/enable-logic-app.png)

## <a name="delete-your-logic-app"></a>Eliminación de la aplicación lógica

Para eliminar la aplicación lógica desde Azure Portal, en Cloud Explorer, abra el menú contextual de la aplicación lógica y seleccione **Eliminar**.

![Eliminación de la aplicación lógica](./media/manage-logic-apps-with-visual-studio/delete-logic-app.png)

> [!NOTE]
> Cuando se elimina una aplicación lógica, no se crean instancias de nuevas ejecuciones. Todas las ejecuciones nuevas y pendientes se cancelan. Si tiene miles de ejecuciones, la cancelación puede tardar bastante tiempo en completarse. 

## <a name="troubleshooting"></a>solución de problemas

Cuando se abre el proyecto de aplicación lógica en el Diseñador de aplicaciones lógicas, es posible que no tenga la opción de seleccionar la suscripción a Azure. En cambio, la aplicación lógica se abre con una suscripción a Azure que no es la que quiere usar. Este comportamiento ocurre porque después de abrir el archivo .json de una aplicación lógica, Visual Studio almacena en caché la primera suscripción seleccionada para su uso futuro. Para solucionar este problema, pruebe uno de estos pasos:

* Cambie el nombre del archivo .json de la aplicación lógica. La caché de suscripción depende del nombre del archivo.

* Para eliminar las suscripciones previamente seleccionadas de *todas* las aplicaciones lógicas de la solución, elimine la carpeta de configuración oculta de Visual Studio (.vs) del directorio de la solución. Esta ubicación almacena la información de suscripción.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió a administrar las aplicaciones lógicas implementadas con Visual Studio. A continuación, obtenga información sobre cómo personalizar las definiciones de aplicaciones lógicas para la implementación:

> [!div class="nextstepaction"]
> [Creación de definiciones de aplicaciones lógicas en JSON](../logic-apps/logic-apps-author-definitions.md)
