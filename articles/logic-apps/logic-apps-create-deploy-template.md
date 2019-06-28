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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "62128607"
---
# <a name="create-azure-resource-manager-templates-for-deploying-logic-apps"></a>Creación de plantillas de Azure Resource Manager para implementar aplicaciones lógicas

Cuando crea una aplicación lógica, puede expandir la definición de esta en una [plantilla de Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). Puede usar esta plantilla para automatizar las implementaciones definiendo los recursos y parámetros que desea usar para la implementación y proporcionando los valores de los parámetros mediante un [archivo de parámetros](../azure-resource-manager/resource-group-template-deploy.md#parameter-files).
De este modo, puede implementar aplicaciones lógicas con más facilidad en cualquier entorno o en el grupo de recursos de Azure que desee. 

Azure Logic Apps proporciona una [plantilla precompilada de Azure Resource Manager para aplicaciones lógicas](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json) que puede reutilizar no solo para crear aplicaciones lógicas, sino también para definir los recursos y parámetros que desea usar para la implementación. Puede usar esta plantilla para sus propios escenarios empresariales o personalizar para satisfacer sus requisitos. Más información sobre la [sintaxis y estructura de las plantillas de Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md). Para conocer la sintaxis y las propiedades JSON, consulte [Microsoft.Logic resource types](/azure/templates/microsoft.logic/allversions) (Tipos de recursos de Microsoft.Logic).

Para más información acerca de las plantillas de Azure Resource Manager, consulte estos artículos:

* [Creación de plantillas del Administrador de recursos de Azure](../azure-resource-manager/resource-group-authoring-templates.md)
* [Desarrollo de plantillas de Azure Resource Manager para mantener la coherencia en la nube](../azure-resource-manager/templates-cloud-consistency.md)

## <a name="logic-app-structure"></a>Estructura de una aplicación lógica

La definición de una aplicación lógica tiene estas secciones básicas que se pueden ver cambiando de la "vista Diseñador" a la "vista Código" o mediante una herramienta como [Azure Resource Explorer](http://resources.azure.com). Las definiciones de aplicaciones lógicas utilizan notación de objetos JavaScript (JSON), así que para más información sobre la sintaxis y las propiedades de JSON, consulte [Tipos de recursos de Microsoft.Logic](/azure/templates/microsoft.logic/allversions).

* **Recurso de aplicación lógica**: describe información como la ubicación o región, el plan de precios y la definición del flujo de trabajo de la aplicación lógica.

* **Definición del flujo de trabajo**: describe los desencadenadores y acciones de la aplicación lógica y cómo ejecuta el servicio Azure Logic Apps el flujo de trabajo. En la vista Código, puede encontrar la definición de flujo de trabajo en la sección `definition`.

* **Conexiones**: Si emplea conectores administrados en la aplicación lógica, la sección `$connections` hace referencia a otros recursos que almacenan de forma segura metadatos sobre estas conexiones entre la aplicación lógica y los demás sistemas o servicios, como cadenas de conexión y tokens de acceso. En la definición de la aplicación lógica, las referencias a estas conexiones aparecen en la sección `parameters`.

Para crear una plantilla de aplicación lógica que pueda utilizar con implementaciones de grupos de recursos de Azure, es preciso definir los recursos y parametrizarlos según sea necesario. Por ejemplo, si va a implementar en un entorno de desarrollo, prueba y producción, probablemente deseará usar cadenas de conexión diferentes para una base de datos SQL en cada entorno.
O bien, puede que desee realizar la implementación en diferentes suscripciones o grupos de recursos.

## <a name="create-logic-app-deployment-templates"></a>Creación de plantillas de implementación de aplicación lógica

Para conocer la manera más fácil de crear una plantilla válida de implementación de aplicación lógica, use Visual Studio y la extensión Herramientas de Azure Logic Apps para Visual Studio. Al descargar la aplicación lógica desde Azure Portal en Visual Studio, obtiene una plantilla de implementación válida que puede usar con todas las suscripciones y ubicaciones de Azure. Además, al descargar la aplicación lógica, automáticamente se parametriza la definición de la aplicación lógica que está insertada en la plantilla.
Para más información sobre la creación y administración de aplicaciones lógicas en Visual Studio, consulte [Creación de aplicaciones lógicas con Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) y [Administración de aplicaciones lógicas con Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md).

Además de Visual Studio o de la creación manual de la plantilla con los parámetros necesarios mediante las instrucciones de este tema, también puede usar el [módulo de PowerShell para crear plantillas de aplicaciones lógicas](https://github.com/jeffhollan/LogicAppTemplateCreator). Este módulo de código abierto evalúa primero la aplicación lógica y las conexiones que usa. Después, el módulo genera los recursos de plantilla con los parámetros necesarios para la implementación. Por ejemplo, suponga que tiene una aplicación lógica que recibe un mensaje de una cola de Azure Service Bus y agrega datos a una base de datos de Azure SQL. La herramienta del módulo conserva toda la lógica de orquestación y parametriza las cadenas de conexión de SQL y Service Bus con el fin de que se puedan establecer esos valores en la implementación.

> [!IMPORTANT]
> Las conexiones deben estar dentro del mismo grupo de recursos de Azure que la aplicación lógica.
> Para que el módulo de PowerShell funcione con cualquier token de acceso de inquilino y suscripción de Azure, use el módulo con la [herramienta cliente de Azure Resource Manager](https://github.com/projectkudu/ARMClient). Para más información, consulte este [artículo acerca de la herramienta cliente de Azure Resource Manager](https://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) que analiza ARMClient con más detalle.

### <a name="install-powershell-module-for-logic-app-templates"></a>Instalación del módulo de PowerShell para plantillas de aplicación lógica

La forma más fácil de instalar el módulo es desde la [Galería de PowerShell](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1). Para ello, use este comando:

`Install-Module -Name LogicAppTemplate`

También puede instalar el módulo de PowerShell manualmente:

1. Descargue la versión más reciente de [Logic App Template Creator](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).

1. Extraiga la carpeta en su carpeta del módulo de PowerShell (normalmente es `%UserProfile%\Documents\WindowsPowerShell\Modules`).

### <a name="generate-logic-app-template---powershell"></a>Generación de una plantilla de aplicación lógica: PowerShell

Después de instalar PowerShell, puede generar una plantilla mediante el comando siguiente:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId` es el identificador de la suscripción de Azure. Esta línea primero obtiene acceso al token mediante ARMClient, luego lo canaliza hacia el script de PowerShell y finalmente crea la plantilla en un archivo JSON.

## <a name="parameters-in-logic-app-templates"></a>Parámetros de las plantillas de aplicación lógica

Después de crear la plantilla de aplicación lógica, puede agregar y editar todos los parámetros necesarios. La plantilla tiene más de una sección `parameters`, por ejemplo: 

* La definición del flujo de trabajo de la aplicación lógica tiene su propia [sección `parameters`](../logic-apps/logic-apps-workflow-definition-language.md#parameters) en la que puede definir todos los parámetros que usa la aplicación lógica para aceptar entradas en la implementación.

* La plantilla de Resource Manager tiene su propia [sección `parameters`](../azure-resource-manager/resource-group-authoring-templates.md#parameters), independiente de la sección `parameters` de la aplicación lógica. Por ejemplo:

  [!INCLUDE [logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

Por ejemplo, suponga que la definición de su aplicación lógica hace referencia a un identificador de recurso que representa a una función de Azure o un flujo de trabajo de aplicación lógica anidada, y desea implementar ese identificador de recurso junto con la aplicación lógica como una única implementación. Puede agregar ese identificador como un recurso en la plantilla y parametrizarlo. Puede usar este mismo enfoque para hacer referencia a API personalizadas o a puntos de conexión de OpenAPI (anteriormente "Swagger") que desea implementar con cada grupo de recursos de Azure.

Cuando use parámetros en la plantilla de implementación, siga esta guía para que el diseñador de Logic Apps pueda mostrar esos parámetros correctamente:

* Use parámetros solo en estos desencadenadores y acciones:

  * Aplicación Azure Functions
  * Flujo de trabajo de aplicación lógica anidada o secundaria
  * Llamada a API Management
  * Dirección URL en tiempo de ejecución de conexión de API
  * Ruta de conexión de API

* Cuando defina parámetros, asegúrese de que proporciona los valores predeterminados mediante el valor de propiedad `defaultValue`, por ejemplo:

  ```json
  "parameters": {
     "IntegrationAccount": {
        "type":"string",
        "minLength": 1,
        "defaultValue": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource=group-name>/providers/Microsoft.Logic/integrationAccounts/<integration-account-name>"
     }
  },
  ```

* Para proteger u ocultar información confidencial en las plantillas, proteja los parámetros. Más información sobre [cómo usar parámetros protegidos](../logic-apps/logic-apps-securing-a-logic-app.md#secure-parameters-workflow).

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

### <a name="reference-dependent-resources"></a>Referencias a recursos dependientes

Si la aplicación lógica requiere referencias a recursos dependientes, puede usar las [funciones de plantilla de Azure Resource Manager](../azure-resource-manager/resource-group-template-functions.md) en la plantilla de implementación de la aplicación lógica. Por ejemplo, suponga que desea que la aplicación lógica haga referencia a una función o cuenta de integración de Azure con definiciones para asociados, acuerdos y otros artefactos que desea implementar junto con la aplicación lógica.
Puede usar las funciones de la plantilla de Resource Manager, como `parameters`, `variables`, `resourceId`, `concat`, etc.

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

Y aquí se indica cómo usar estos parámetros al hacer referencia a la función de Azure:

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

## <a name="add-logic-app-to-resource-group-project"></a>Agregar la aplicación lógica a un proyecto de grupo de recursos

Si tiene un proyecto de grupo de recursos de Azure existente, puede agregar la aplicación lógica mediante la ventana Esquema JSON. También puede agregar otra aplicación lógica junto con la que creara anteriormente.

1. En el Explorador de soluciones, abra el archivo `<template>.json`.

2. En el menú **Vista**, seleccione **Otras ventanas** > **Esquema JSON**.

3. Para agregar un recurso al archivo de plantilla, seleccione **Agregar recurso** en la parte superior de la ventana Esquema JSON. O, en la ventana Esquema de JSON, haga clic en **recursos** y seleccione **Agregar nuevo recurso**.

   ![Ventana Esquema JSON](./media/logic-apps-create-deploy-template/jsonoutline.png)

4. En el cuadro de diálogo **Agregar recurso**, busque y seleccione **Aplicación lógica**. Asigne un nombre a la aplicación lógica y elija **Agregar**.

   ![Agregar recurso](./media/logic-apps-create-deploy-template/addresource.png)

## <a name="get-support"></a>Obtención de soporte técnico

Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Implementación de plantillas de aplicaciones lógicas](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md)
