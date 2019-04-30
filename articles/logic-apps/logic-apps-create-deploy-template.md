---
title: Creación de plantillas de implementación para Azure Logic Apps | Microsoft Docs
description: Creación de plantillas de Azure Resource Manager para implementar aplicaciones lógicas
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.assetid: 85928ec6-d7cb-488e-926e-2e5db89508ee
ms.date: 10/18/2016
ms.openlocfilehash: 624539557b0bf57e9d919a3a46337f1cf93a4f07
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128607"
---
# <a name="create-azure-resource-manager-templates-for-deploying-logic-apps"></a>Creación de plantillas de Azure Resource Manager para implementar aplicaciones lógicas

Cuando se crea una aplicación lógica, puede expandir la definición de la aplicación lógica en un [plantilla de Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). A continuación, puede usar esta plantilla para automatizar las implementaciones mediante la definición de los recursos y los parámetros que desea utilizar para la implementación y proporcionar los valores de parámetro a través de un [archivo de parámetros](../azure-resource-manager/resource-group-template-deploy.md#parameter-files).
De este modo, puede implementar aplicaciones lógicas con más facilidad y a cualquier entorno o el grupo de recursos de Azure que desea. 

Azure Logic Apps proporciona un [plantilla de Azure Resource Manager de aplicaciones lógicas precompiladas](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json) que puede reutilizar, no solo para la creación de aplicaciones lógicas, sino también para definir los recursos y los parámetros que se usarán para la implementación. Puede usar esta plantilla para sus propios escenarios empresariales o personalizar para satisfacer sus requisitos. Obtenga más información sobre [estructura de plantilla de Resource Manager y la sintaxis](../azure-resource-manager/resource-group-authoring-templates.md). Para conocer la sintaxis y las propiedades JSON, consulte [Microsoft.Logic resource types](/azure/templates/microsoft.logic/allversions) (Tipos de recursos de Microsoft.Logic).

Para obtener más información acerca de las plantillas de Azure Resource Manager, consulte estos artículos:

* [Creación de plantillas del Administrador de recursos de Azure](../azure-resource-manager/resource-group-authoring-templates.md)
* [Desarrollar plantillas de Azure Resource Manager para mantener la coherencia en la nube](../azure-resource-manager/templates-cloud-consistency.md)

## <a name="logic-app-structure"></a>Estructura de la aplicación lógica

La definición de aplicación lógica tiene estas secciones básicas, que se pueden ver mediante el cambio de "vista de diseñador" a "vista de código" o mediante una herramienta como [Azure Resource Explorer](http://resources.azure.com). Definiciones de aplicación lógica Javascript Object Notation (JSON), así que para obtener más información sobre la sintaxis de JSON y propiedades, consulte [tipos de recursos de Microsoft.Logic](/azure/templates/microsoft.logic/allversions).

* **Recurso de aplicación lógica**: Describe la información como la ubicación o región, de la aplicación lógica, plan de precios y definición de flujo de trabajo.

* **Definición del flujo de trabajo**: Describe los desencadenadores y acciones y cómo el servicio de Azure Logic Apps ejecuta el flujo de trabajo de la aplicación lógica. En la vista de código, puede encontrar la definición de flujo de trabajo en el `definition` sección.

* **Conexiones**: Si usa conectores administrados en la aplicación lógica, el `$connections` sección hace referencia a otros recursos que almacenan metadatos acerca de estas conexiones entre la aplicación lógica y otros sistemas o servicios, como las cadenas de conexión y los tokens de acceso de manera segura. Dentro de la definición de aplicación lógica, las referencias a estas conexiones aparecen dentro de la definición de aplicación lógica `parameters` sección.

Para crear una plantilla de aplicación lógica que puede usar con las implementaciones de grupo de recursos de Azure, debe definir los recursos y parametrizar según sea necesario. Por ejemplo, si va a implementar en un entorno de desarrollo, prueba y producción, probablemente deseará usar cadenas de conexión diferentes para una base de datos SQL en cada entorno.
O bien, puede que desee realizar la implementación en diferentes suscripciones o grupos de recursos.

## <a name="create-logic-app-deployment-templates"></a>Crear plantillas de implementación de aplicación de lógica

Para que la manera más fácil de crear una plantilla de implementación de aplicación lógica válida, use Visual Studio y las herramientas de Azure Logic Apps para la extensión de Visual Studio. Al descargar la aplicación lógica desde el portal de Azure en Visual Studio, obtenga una plantilla de implementación válido que puede usar con cualquier suscripción de Azure y la ubicación. Además, parametriza la definición de aplicación lógica que está incrustada en la plantilla de descargar automáticamente la aplicación lógica.
Para obtener más información sobre cómo crear y administrar aplicaciones lógicas en Visual Studio, consulte [crear aplicaciones lógicas con Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) y [administrar aplicaciones lógicas con Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md).

Aparte de Visual Studio o crear manualmente la plantilla y los parámetros necesarios, siga las instrucciones de este tema, también puede usar el [módulo de PowerShell para crear plantillas de aplicaciones de lógica](https://github.com/jeffhollan/LogicAppTemplateCreator). Este módulo de código abierto evalúa primero la aplicación lógica y las conexiones que usa la aplicación lógica. El módulo, a continuación, genera los recursos de plantilla con los parámetros necesarios para la implementación. Por ejemplo, suponga que tiene una aplicación lógica que recibe un mensaje de una cola de Azure Service Bus y agrega datos a una base de datos SQL de Azure. La herramienta de módulo conserva toda la lógica de orquestación y parametriza las cadenas de conexión SQL y Service Bus para que pueda establecer esos valores en la implementación.

> [!IMPORTANT]
> Las conexiones deben existir en el mismo grupo de recursos de Azure que la aplicación lógica.
> Para el módulo de PowerShell trabajar con cualquier inquilino y suscripción access token, uso de Azure, el módulo con el [herramienta de cliente de Azure Resource Manager](https://github.com/projectkudu/ARMClient). Para obtener más información, consulte este [artículo acerca de la herramienta de cliente de Azure Resource Manager](https://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) describe ARMClient con más detalle.

### <a name="install-powershell-module-for-logic-app-templates"></a>Instalar el módulo de PowerShell para plantillas de aplicaciones lógicas

Para instalar el módulo de la manera más fácil la [Galería de PowerShell](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1), use este comando:

`Install-Module -Name LogicAppTemplate`

Puede instalar manualmente el módulo de PowerShell:

1. Descargue la última versión [Logic App Template Creator](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).

1. Extraiga la carpeta en la carpeta de módulo de PowerShell, que suele ser `%UserProfile%\Documents\WindowsPowerShell\Modules`.

### <a name="generate-logic-app-template---powershell"></a>Generar plantilla de aplicación lógica - PowerShell

Después de instalar PowerShell, puede generar una plantilla mediante el comando siguiente:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId` es el identificador de la suscripción de Azure. Esta línea primero obtiene acceso al token mediante ARMClient, luego lo canaliza hacia el script de PowerShell y finalmente crea la plantilla en un archivo JSON.

## <a name="parameters-in-logic-app-templates"></a>Parámetros de plantillas de aplicaciones lógicas

Después de crear la plantilla de aplicación lógica, puede agregar y editar todos los parámetros necesarios. La plantilla tiene más de un `parameters` sección, por ejemplo: 

* Definición de flujo de trabajo de la aplicación lógica tiene su propio [ `parameters` sección](../logic-apps/logic-apps-workflow-definition-language.md#parameters) donde puede definir todos los parámetros que utiliza la aplicación lógica para aceptar entradas en la implementación.

* La plantilla de Resource Manager tiene su propio [ `parameters` sección](../azure-resource-manager/resource-group-authoring-templates.md#parameters), independiente de la aplicación lógica `parameters` sección. Por ejemplo: 

  [!INCLUDE [logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

Por ejemplo, suponga que la definición de aplicación lógica haga referencia a un identificador de recurso que representa una función de Azure o un flujo de trabajo de la aplicación lógica anidada, y desea implementar ese identificador de recurso, junto con la aplicación lógica como una sola implementación. Puede agregar ese identificador como un recurso en la plantilla y parametrizar ese identificador. Puede usar este mismo enfoque para las referencias a los puntos de conexión de OpenAPI o la API personalizados (anteriormente "Swagger") que desea implementar con cada grupo de recursos de Azure.

Al usar parámetros en la plantilla de implementación, siga estas instrucciones para que el Diseñador de Logic Apps puede mostrar correctamente esos parámetros:

* Usar parámetros solo en estos desencadenadores y acciones:

  * Aplicación de Azure Functions
  * Anidados o flujo de trabajo de aplicación de lógica de secundarios
  * Llamada de API Management
  * Dirección URL en tiempo de ejecución de conexión de API
  * Ruta de conexión de API

* Al definir parámetros, asegúrese de que proporcione los valores predeterminados mediante la `defaultValue` valor de propiedad, por ejemplo:

  ```json
  "parameters": {
     "IntegrationAccount": {
        "type":"string",
        "minLength": 1,
        "defaultValue": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource=group-name>/providers/Microsoft.Logic/integrationAccounts/<integration-account-name>"
     }
  },
  ```

* Para proteger u ocultar información confidencial en las plantillas, proteja sus parámetros. Obtenga más información sobre [cómo usar parámetros protegidos](../logic-apps/logic-apps-securing-a-logic-app.md#secure-parameters-workflow).

Este es un ejemplo que muestra cómo se puede parametrizar la acción "Enviar mensaje" de Azure Service Bus:

```json
"Send_message": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "connection": {
            "name": "@parameters('$connections')['servicebus']['connectionId']"
         },
         // If the `host.runtimeUrl` property appears in your template, 
         // you can remove this property, which is optional, for example:
         "runtimeUrl": {}
      },
      "method": "POST",
      "path": "[concat('/@{encodeURIComponent(''', parameters('<Azure-Service-Bus-queue-name>'), ''')}/messages')]",
      "body": {
         "ContentData": "@{base64(triggerBody())}"
      },
      "queries": {
         "systemProperties": "None"
      }
   },
   "runAfter": {}
},
```

### <a name="reference-dependent-resources"></a>Recursos dependientes de referencia

Si la aplicación lógica requiere referencias a los recursos dependientes, puede usar [funciones de plantilla de Azure Resource Manager](../azure-resource-manager/resource-group-template-functions.md) en la plantilla de implementación de la aplicación lógica. Por ejemplo, suponga que desea que la aplicación lógica para hacer referencia a una función de Azure o una cuenta de integración con las definiciones para socios, acuerdos y otros artefactos que desee implementadas junto con la aplicación lógica.
Puede usar las funciones de plantilla de Resource Manager, como `parameters`, `variables`, `resourceId`, `concat`, y así sucesivamente.

Este es un ejemplo que muestra cómo se puede reemplazar el identificador de recurso para una función de Azure mediante la definición de estos parámetros:

``` json
"parameters": {
   "<Azure-function-name>": {
      "type": "string",
      "minLength": 1,
      "defaultValue": "<Azure-function-name>"
   }
},
```

Le mostramos cómo usar estos parámetros cuando se hace referencia a la función de Azure:

```json
"MyFunction": {
   "type": "Function",
   "inputs": {
      "body": {},
      "function": {
         "id":"[resourceid('Microsoft.Web/sites/functions','<Azure-Functions-app-name>', parameters('<Azure-function-name>'))]"
      }
   },
   "runAfter": {}
},
```

## <a name="add-logic-app-to-resource-group-project"></a>Agregar la aplicación lógica al proyecto del grupo de recursos

Si tiene un proyecto de grupo de recursos de Azure existente, puede agregar la aplicación lógica en el proyecto mediante el uso de la ventana Esquema JSON. También puede agregar otra aplicación lógica junto con la que creara anteriormente.

1. En el Explorador de soluciones, abra el archivo `<template>.json`.

2. Desde el **vista** menú, seleccione **Other Windows** > **esquema JSON**.

3. Para agregar un recurso al archivo de plantilla, elija **Agregar recurso** en la parte superior de la ventana Esquema JSON. O, en la ventana Esquema de JSON, haga clic en **recursos** y seleccione **Agregar nuevo recurso**.

   ![Ventana Esquema JSON](./media/logic-apps-create-deploy-template/jsonoutline.png)

4. En el cuadro de diálogo **Agregar recurso**, busque y seleccione **Aplicación lógica**. Asigne un nombre a la aplicación lógica y elija **Agregar**.

   ![Agregar recurso](./media/logic-apps-create-deploy-template/addresource.png)

## <a name="get-support"></a>Obtención de soporte técnico

Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Implementación de plantillas de aplicación lógica](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md)
