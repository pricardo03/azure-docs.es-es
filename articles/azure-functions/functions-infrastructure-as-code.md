---
title: Automatización de la implementación de recursos para una aplicación de función en Azure Functions | Microsoft Docs
description: Obtenga información sobre cómo crear una plantilla de Azure Resource Manager que implemente su aplicación de función.
services: Functions
documtationcenter: na
author: ggailey777
manager: jeconnoc
keywords: azure functions, funciones, arquitectura sin servidor, infraestructura como código, azure resource manager
ms.assetid: d20743e3-aab6-442c-a836-9bcea09bfd32
ms.service: azure-functions
ms.server: functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: glenga
ms.openlocfilehash: 5d028768c062ef7df74d48f83ccc4e27a506f1ac
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59270911"
---
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a>Automatización de la implementación de recursos para una aplicación de función en Azure Functions

Puede usar una plantilla de Azure Resource Manager para implementar una aplicación de función. En este artículo se describen los recursos y los parámetros necesarios para hacerlo. Es posible que tenga que implementar recursos adicionales, dependiendo de los [desencadenadores y enlaces](functions-triggers-bindings.md) de la aplicación de función.

Para más información sobre la creación de plantillas, consulte [Creación de plantillas de Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

Para obtener las plantillas de ejemplo, vea:
- [Aplicación de función en el plan de consumo]
- [Aplicación de función en el plan de App Service]

> [!NOTE]
> El plan Premium para el hospedaje de Azure Functions está actualmente en versión preliminar. Para obtener más información, consulte [plan Premium de Azure Functions](functions-premium-plan.md).

## <a name="required-resources"></a>Recursos necesarios

Normalmente, una implementación de Azure Functions consta de estos recursos:

| Recurso                                                                           | Requisito | Referencia de sintaxis y las propiedades                                                         |   |
|------------------------------------------------------------------------------------|-------------|-----------------------------------------------------------------------------------------|---|
| Una aplicación de función.                                                                     | Obligatorio    | [Microsoft.Web/sites](/azure/templates/microsoft.web/sites)                             |   |
| Una cuenta de [Azure Storage](../storage/index.yml).                                   | Obligatorio    | [Microsoft.Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts) |   |
| Un [Application Insights](../azure-monitor/app/app-insights-overview.md) componente | Opcional    | [Microsoft.Insights/components](/azure/templates/microsoft.insights/components)         |   |
| Un [plan de hospedaje](./functions-scale.md)                                             | Opcional<sup>1</sup>    | [Microsoft.Web/serverfarms](/azure/templates/microsoft.web/serverfarms)                 |   |

<sup>1</sup>un plan de hospedaje solo es necesario si decide ejecutar la aplicación de función en un [plan Premium](./functions-premium-plan.md) (en versión preliminar) o en un [plan de App Service](../app-service/overview-hosting-plans.md).

> [!TIP]
> Aunque no es necesario, se recomienda encarecidamente que configurar Application Insights para su aplicación.

<a name="storage"></a>
### <a name="storage-account"></a>Cuenta de almacenamiento

Se necesita una cuenta de Azure Storage para una aplicación de función. Se necesita una cuenta de uso general que admita blobs, tablas, colas y archivos. Para más información, vea [Requisitos de la cuenta de almacenamiento de Azure Functions](functions-create-function-app-portal.md#storage-account-requirements).

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2018-07-01",
    "location": "[resourceGroup().location]",
    "kind": "StorageV2",
    "properties": {
        "accountType": "[parameters('storageAccountType')]"
    }
}
```

Además, la propiedad `AzureWebJobsStorage` se debe especificar como una configuración de aplicación en la configuración del sitio. Si la aplicación de función no usa Application Insights para la supervisión, debe especificar también `AzureWebJobsDashboard` como configuración de aplicación.

El tiempo de ejecución de Azure Functions usa la cadena de conexión `AzureWebJobsStorage` para crear colas internas.  Cuando Application Insights no está habilitado, el entorno de tiempo de ejecución usa la cadena de conexión `AzureWebJobsDashboard` para iniciar sesión en Azure Table Storage y activar la pestaña **Supervisión** en el portal.

Estas propiedades se especifican en la colección `appSettings` del objeto `siteConfig`:

```json
"appSettings": [
    {
        "name": "AzureWebJobsStorage",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    },
    {
        "name": "AzureWebJobsDashboard",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    }
]
```

### <a name="application-insights"></a>Application Insights

Se recomienda Application Insights para supervisar las aplicaciones de función. El recurso de Application Insights se define con el tipo **Microsoft.Insights/Components** y el tipo **web**:

```json
        {
            "apiVersion": "2015-05-01",
            "name": "[variables('appInsightsName')]",
            "type": "Microsoft.Insights/components",
            "kind": "web",
            "location": "[resourceGroup().location]",
            "tags": {
                "[concat('hidden-link:', resourceGroup().id, '/providers/Microsoft.Web/sites/', variables('functionAppName'))]": "Resource"
            },
            "properties": {
                "Application_Type": "web",
                "ApplicationId": "[variables('functionAppName')]"
            }
        },
```

Además, debe proporcionarse a la aplicación de función con la clave de instrumentación del `APPINSIGHTS_INSTRUMENTATIONKEY` configuración de la aplicación. Esta propiedad se especifica en el `appSettings` colección en el `siteConfig` objeto:

```json
"appSettings": [
    {
        "name": "APPINSIGHTS_INSTRUMENTATIONKEY",
        "value": "[reference(resourceId('microsoft.insights/components/', variables('appInsightsName')), '2015-05-01').InstrumentationKey]"
    }
]
```

### <a name="hosting-plan"></a>Plan de hospedaje

La definición del plan de hospedaje varía y puede ser uno de los siguientes:
* [Plan de consumo](#consumption) (valor predeterminado)
* [Plan Premium](#premium) (en versión preliminar)
* [Plan de App Service](#app-service-plan)

### <a name="function-app"></a>Aplicación de función

El recurso de aplicación de función se define mediante el uso de un recurso de tipo **Microsoft.Web/Sites** y tipo **functionapp**:

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Insights/components', variables('appInsightsName'))]"
    ]
```

> [!IMPORTANT]
> Si va a definir explícitamente un plan de hospedaje, se necesitaría un elemento adicional de la matriz de dependsOn: `"[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"`

Una aplicación de función debe incluir esta configuración de aplicación:

| Nombre del valor                 | DESCRIPCIÓN                                                                               | Valores de ejemplo                        |
|------------------------------|-------------------------------------------------------------------------------------------|---------------------------------------|
| AzureWebJobsStorage          | Una cadena de conexión a un almacenamiento de la cuenta que el tiempo de ejecución de funciones para la cola interna | Consulte [cuenta de almacenamiento](#storage)       |
| FUNCTIONS_EXTENSION_VERSION  | La versión del runtime de Azure Functions                                                | `~2`                                  |
| FUNCTIONS_WORKER_RUNTIME     | La pila de lenguaje que se usará para las funciones de esta aplicación                                   | `dotnet`, `node`, `java`, o `python` |
| WEBSITE_NODE_DEFAULT_VERSION | Solo es necesario si usa el `node` pila de lenguaje, especifica la versión para usar              | `10.14.1`                             |

Estas propiedades se especifican en el `appSettings` colección en el `siteConfig` propiedad:

```json
"properties": {
    "siteConfig": {
        "appSettings": [
            {
                "name": "AzureWebJobsStorage",
                "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
            },
            {
                "name": "FUNCTIONS_WORKER_RUNTIME",
                "value": "node"
            },
            {
                "name": "WEBSITE_NODE_DEFAULT_VERSION",
                "value": "10.14.1"
            },
            {
                "name": "FUNCTIONS_EXTENSION_VERSION",
                "value": "~2"
            }
        ]
    }
}
```

<a name="consumption"></a>

## <a name="deploy-on-consumption-plan"></a>Implantar en un plan de consumo

El plan de consumo asigna automáticamente capacidad de proceso cuando se ejecuta el código, se amplía horizontalmente cuando es necesario para gestionar la carga y se reduce horizontalmente cuando no se ejecuta código. No tiene que pagar por máquinas virtuales inactivas y no tiene que reservar capacidad de antemano. Para obtener más información, consulte [escalado y hospedaje Azure Functions](functions-scale.md#consumption-plan).

Para obtener una plantilla de Azure Resource Manager de ejemplo, vea [Aplicación de función en el plan de consumo].

### <a name="create-a-consumption-plan"></a>Crear un plan de consumo

No necesita definir un plan de consumo. Uno se automáticamente crea ni selecciona por región cuando se crea el propio recurso de aplicación de función.

El plan de consumo es un tipo especial de recurso "granja de servidores". Para Windows, se puede especificar mediante el `Dynamic` valor para el `computeMode` y `sku` propiedades:

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "computeMode": "Dynamic",
        "sku": "Dynamic"
    }
}
```

> [!NOTE]
> El plan de consumo no se puede definir explícitamente para Linux. Se crearán automáticamente.

Si se define explícitamente su plan de consumo, deberá establecer el `serverFarmId` propiedad en la aplicación de forma que apunte al identificador de recurso del plan. Debe asegurarse de que la aplicación de función tiene un `dependsOn` establecer para el plan también.

### <a name="create-a-function-app"></a>Creación de una aplicación de función

#### <a name="windows"></a> Windows

En Windows, un plan de consumo requiere dos configuraciones adicionales en la configuración del sitio: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` y `WEBSITE_CONTENTSHARE`. Estas propiedades configuran la cuenta de almacenamiento y la ruta de acceso del archivo donde se almacenan el código y la configuración de la aplicación de función.

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTSHARE",
                    "value": "[toLower(variables('functionAppName'))]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        }
    }
}
```

#### <a name="linux"></a>Linux

En Linux, debe tener la aplicación de función su `kind` establecido en `functionapp,linux`, y debe tener la `reserved` propiedad establecida en `true`:

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp,linux",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountName'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        },
        "reserved": true
    }
}
```



<a name="premium"></a>

## <a name="deploy-on-premium-plan"></a>Implantar en un plan Premium

El plan Premium ofrece la misma escala, como el plan de consumo, pero incluye funcionalidades adicionales y recursos dedicados. Para obtener más información, consulte [Plan de Azure Functions Premium (versión preliminar)](./functions-premium-plan.md).

### <a name="create-a-premium-plan"></a>Crear un plan Premium

Un plan Premium es un tipo especial de recurso "granja de servidores". Puede especificar mediante el uso `EP1`, `EP2`, o `EP3` para el `sku` valor de propiedad.

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "sku": "EP1"
    }
}
```

### <a name="create-a-function-app"></a>Creación de una aplicación de función

Debe tener una aplicación de función en un plan Premium la `serverFarmId` propiedad establecida en el identificador de recurso del plan que creó anteriormente. Además, un plan Premium requiere dos configuraciones adicionales en la configuración del sitio: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` y `WEBSITE_CONTENTSHARE`. Estas propiedades configuran la cuenta de almacenamiento y la ruta de acceso del archivo donde se almacenan el código y la configuración de la aplicación de función.

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",            
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTSHARE",
                    "value": "[toLower(variables('functionAppName'))]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        }
    }
}
```


<a name="app-service-plan"></a> 

## <a name="deploy-on-app-service-plan"></a>Implantar en un plan de App Service

En el plan de App Service, la aplicación de función se ejecuta en máquinas virtuales dedicadas en las SKU de los niveles Básico, Estándar y Premium, de un modo similar a las aplicaciones web. Para más información acerca del funcionamiento del plan de App Service, consulte [Introducción detallada sobre los planes de Azure App Service](../app-service/overview-hosting-plans.md).

Para obtener una plantilla de Azure Resource Manager de ejemplo, vea [Aplicación de función en el plan de App Service].

### <a name="create-an-app-service-plan"></a>Creación de un plan de App Service

Un plan de App Service se define mediante un recurso de "granja de servidores".

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "sku": "[parameters('sku')]",
        "workerSize": "[parameters('workerSize')]",
        "hostingEnvironment": "",
        "numberOfWorkers": 1
    }
}
```

Para ejecutar la aplicación en Linux, también debe establecer el `kind` a `Linux`:

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "kind": "Linux",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "sku": "[parameters('sku')]",
        "workerSize": "[parameters('workerSize')]",
        "hostingEnvironment": "",
        "numberOfWorkers": 1
    }
}
```

### <a name="create-a-function-app"></a>Creación de una aplicación de función 

Una aplicación de función en un plan de App Service debe tener el `serverFarmId` propiedad establecida en el identificador de recurso del plan que creó anteriormente.

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        }
    }
}
```

Las aplicaciones de Linux también deberían incluir una `linuxFxVersion` propiedad bajo `siteConfig`. Si solo va a implementar código, el valor viene determinada por la pila en tiempo de ejecución deseado:

| Pila            | Valor de ejemplo                                         |
|------------------|-------------------------------------------------------|
| Python (versión preliminar) | `DOCKER|microsoft/azure-functions-python3.6:2.0`      |
| JavaScript       | `DOCKER|microsoft/azure-functions-node8:2.0`          |
| .NET             | `DOCKER|microsoft/azure-functions-dotnet-core2.0:2.0` |

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ],
            "linuxFxVersion": "DOCKER|microsoft/azure-functions-node8:2.0"
        }
    }
}
```

Si estás [implementar una imagen de contenedor personalizado](./functions-create-function-linux-custom-image.md), debe especificarlo con `linuxFxVersion` e incluyen la configuración que permite que la imagen que se puede extraer, como en [Web Apps for Containers](/azure/app-service/containers). Además, establezca `WEBSITES_ENABLE_APP_SERVICE_STORAGE` a `false`, ya que el contenido de la aplicación se proporciona en el propio contenedor:

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                },
                {
                    "name": "DOCKER_REGISTRY_SERVER_URL",
                    "value": "[parameters('dockerRegistryUrl')]"
                },
                {
                    "name": "DOCKER_REGISTRY_SERVER_USERNAME",
                    "value": "[parameters('dockerRegistryUsername')]"
                },
                {
                    "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
                    "value": "[parameters('dockerRegistryPassword')]"
                },
                {
                    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
                    "value": "false"
                }
            ],
            "linuxFxVersion": "DOCKER|myacr.azurecr.io/myimage:mytag"
        }
    }
}
```

## <a name="customizing-a-deployment"></a>Personalizar una implementación

Una aplicación de función tiene muchos recursos secundarios que puede usar en la implementación, incluidas la configuración de la aplicación y las opciones de control del código fuente. Es posible que también elija quitar el recurso secundario **sourcecontrols** y usar otra [opción de implementación](functions-continuous-deployment.md) en su lugar.

> [!IMPORTANT]
> Para implementar la aplicación de forma correcta mediante Azure Resource Manager, es importante comprender cómo se implementan los recursos en Azure. En el siguiente ejemplo, se aplican configuraciones de nivel superior mediante **siteConfig**. Es importante establecer estas configuraciones en un nivel superior porque transmiten información al motor de implementación y en tiempo de ejecución de Functions. Se necesita información de nivel superior antes de aplicar el recurso secundario **sourcecontrols/web**. Aunque es posible configurar estas opciones en el recurso de nivel secundario **config/appSettings**, en algunos casos debe implementar la aplicación de función *antes de* que se aplique **config/appSettings**. Por ejemplo, cuando se usan funciones con [Logic Apps](../logic-apps/index.yml), las funciones son una dependencia de otro recurso.

```json
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('appName')]",
  "type": "Microsoft.Web/sites",
  "kind": "functionapp",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Web/serverfarms', parameters('appName'))]"
  ],
  "properties": {
     "serverFarmId": "[variables('appServicePlanName')]",
     "siteConfig": {
        "alwaysOn": true,
        "appSettings": [
            {
                "name": "FUNCTIONS_EXTENSION_VERSION",
                "value": "~2"
            },
            {
                "name": "Project",
                "value": "src"
            }
        ]
     }
  },
  "resources": [
     {
        "apiVersion": "2015-08-01",
        "name": "appsettings",
        "type": "config",
        "dependsOn": [
          "[resourceId('Microsoft.Web/Sites', parameters('appName'))]",
          "[resourceId('Microsoft.Web/Sites/sourcecontrols', parameters('appName'), 'web')]",
          "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
        ],
        "properties": {
          "AzureWebJobsStorage": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]",
          "AzureWebJobsDashboard": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]",
          "FUNCTIONS_EXTENSION_VERSION": "~2",
          "FUNCTIONS_WORKER_RUNTIME": "dotnet",
          "Project": "src"
        }
     },
     {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites/', parameters('appName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('sourceCodeRepositoryURL')]",
            "branch": "[parameters('sourceCodeBranch')]",
            "IsManualIntegration": "[parameters('sourceCodeManualIntegration')]"
          }
     }
  ]
}
```
> [!TIP]
> En esta plantilla se usa el valor de configuración de aplicación [Project](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file), que establece el directorio base en el que el motor de implementación de Functions (Kudu) busca código que se pueda implementar. En nuestro repositorio, nuestras funciones están en una subcarpeta de la carpeta **src**. Por tanto, en el ejemplo anterior, se establece el valor de configuración de la aplicación en `src`. Si las funciones se encuentran en la raíz del repositorio, o si no va a implementar desde el control de código fuente, puede quitar este valor de configuración de la aplicación.

## <a name="deploy-your-template"></a>Implementación de la plantilla

Puede usar cualquiera de los siguientes métodos para implementar la plantilla:

* [PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [Azure Portal](../azure-resource-manager/resource-group-template-deploy-portal.md)
* [API DE REST](../azure-resource-manager/resource-group-template-deploy-rest.md)

### <a name="deploy-to-azure-button"></a>Botón Implementación en Azure

Reemplace ```<url-encoded-path-to-azuredeploy-json>``` por una versión [codificada de la URL](https://www.bing.com/search?q=url+encode) de la ruta de acceso sin formato del archivo `azuredeploy.json` en GitHub.

A continuación se muestra un ejemplo que usa Markdown:

```markdown
[![Deploy to Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

A continuación se muestra un ejemplo que usa HTML:

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"></a>
```

## <a name="next-steps"></a>Pasos siguientes

Aprenda a desarrollar y configurar Azure Functions.

* [Referencia para desarrolladores de Azure Functions](functions-reference.md)
* [Cómo configurar la configuración de Azure function app](functions-how-to-use-azure-function-app-settings.md)
* [Cree su primera función de Azure](functions-create-first-azure-function.md)

<!-- LINKS -->

[Aplicación de función en el plan de consumo]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[Aplicación de función en el plan de App Service]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json
