---
title: Cómo seleccionar un destino para versiones en tiempo de ejecución de Azure Functions
description: Azure Functions admite varias versiones del runtime. Obtenga información sobre cómo especificar la versión en tiempo de ejecución de una aplicación de función hospedada en Azure.
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: glenga
ms.openlocfilehash: 2047f11272c6154d4443e889eff24401c2f73afa
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067695"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Cómo seleccionar un destino para versiones en tiempo de ejecución de Azure Functions

Una aplicación de función se ejecuta en una versión específica del runtime de Azure Functions. Hay dos versiones principales: [1.x y 2.x](functions-versions.md). De forma predeterminada, las aplicaciones de función se crean en la versión 2.x del entorno de ejecución. En este artículo se explica cómo configurar una aplicación de función en Azure para que se ejecute en la versión que elija. Para obtener información sobre cómo configurar un entorno de desarrollo local para una versión específica, consulte [Codificación y comprobación de las funciones de Azure Functions en un entorno local](functions-run-local.md).

## <a name="automatic-and-manual-version-updates"></a>Actualizaciones de versiones automáticas y manuales

Azure Functions permite hacer que el destino sea una versión específica del entorno en tiempo de ejecución mediante el uso de la configuración de aplicación `FUNCTIONS_EXTENSION_VERSION` en la aplicación de función. La aplicación de función se mantiene en la versión principal especificada hasta que elija explícitamente moverla a una nueva versión.

Si especifica solo la versión principal ("~ 2" para 2.x o "~1" para 1.x), la aplicación de la función se actualiza automáticamente a las nuevas versiones secundarias del runtime cuando están disponibles. Las nuevas versiones secundarias no introducen cambios importantes. Si especifica una versión secundaria (por ejemplo, "2.0.12345"), la aplicación de función se mantiene en esa versión específica hasta que la modifique explícitamente.

> [!NOTE]
> Si ancla una versión específica de Azure Functions y, a continuación, intenta publicar en Azure mediante Visual Studio, aparecerá un cuadro de diálogo que le solicitará que actualice a la versión más reciente o que cancele la publicación. Para evitarlo, agregue la propiedad `<DisableFunctionExtensionVersionUpdate>true</DisableFunctionExtensionVersionUpdate>` en su archivo `.csproj`.

Cuando una nueva versión está disponible públicamente, un aviso en el portal le da la oportunidad de pasar a esa versión. Después de pasar a una nueva versión, siempre puede usar la configuración de la aplicación `FUNCTIONS_EXTENSION_VERSION` para volver a una versión anterior.

Un cambio en la versión del runtime hace que la aplicación de función se reinicie.

Los valores que puede establecer en la configuración de la aplicación `FUNCTIONS_EXTENSION_VERSION` para habilitar las actualizaciones automáticas son actualmente "~1" para el runtime 1.x y "~2" para 2.x.

## <a name="view-and-update-the-current-runtime-version"></a>Visualización y actualización de la versión actual del entorno de ejecución

Puede cambiar la versión del entorno en tiempo de ejecución que la aplicación de función usa. Debido a la posibilidad de cambios importantes, debería cambiar solo la versión del runtime antes de crear cualquier función en la aplicación de función. Si bien la versión del runtime la determina la configuración `FUNCTIONS_EXTENSION_VERSION`, este cambio se debe hacer en Azure Portal y no cambiando directamente la configuración. Esto se debe a que el portal valida los cambios y hace cambios relacionados adicionales según sea necesario.

### <a name="from-the-azure-portal"></a>Desde Azure Portal

[!INCLUDE [Set the runtime version in the portal](../../includes/functions-view-update-version-portal.md)]

> [!NOTE]
> Azure Portal no permite cambiar la versión del entorno en tiempo de ejecución para una aplicación de función que ya contiene funciones.

### <a name="view-and-update-the-runtime-version-using-azure-cli"></a>Desde la CLI de Azure

`FUNCTIONS_EXTENSION_VERSION` también se puede visualizar y establecer desde la CLI de Azure.

>[!NOTE]
>Dado que otros valores de configuración pueden verse afectados por la versión del entorno de ejecución, debe cambiar la versión en el portal. El portal realiza automáticamente las demás actualizaciones necesarias, como la versión de Node.js y la pila del entorno de ejecución, al cambiar las versiones del entorno de ejecución.  

Mediante la CLI de Azure, puede ver la versión actual del entorno de ejecución con el comando [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings).

```azurecli-interactive
az functionapp config appsettings list --name <function_app> \
--resource-group <my_resource_group>
```

En este código, reemplace `<function_app>` por el nombre de la aplicación de función. Reemplace también `<my_resource_group>` por el nombre del grupo de recursos para la aplicación de función. 

Puede ver el elemento `FUNCTIONS_EXTENSION_VERSION` en la salida siguiente, que se ha truncado para mayor claridad:

```output
[
  {
    "name": "FUNCTIONS_EXTENSION_VERSION",
    "slotSetting": false,
    "value": "~2"
  },
  {
    "name": "FUNCTIONS_WORKER_RUNTIME",
    "slotSetting": false,
    "value": "dotnet"
  },
  
  ...
  
  {
    "name": "WEBSITE_NODE_DEFAULT_VERSION",
    "slotSetting": false,
    "value": "8.11.1"
  }
]
```

Puede actualizar el valor de `FUNCTIONS_EXTENSION_VERSION` en la aplicación de función con el comando [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings).

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```

Reemplace `<function_app>` por el nombre de la aplicación de función. Reemplace también `<my_resource_group>` por el nombre del grupo de recursos para la aplicación de función. Además, reemplace `<version>` por una versión válida del entorno de ejecución 1.x o `~2` en el caso de la versión 2.x.

Este comando se puede ejecutar desde [Azure Cloud Shell](../cloud-shell/overview.md), para lo que es preciso hacer clic en **Pruébelo** en el código de ejemplo anterior. También puede usar la [CLI de Azure localmente](/cli/azure/install-azure-cli) para ejecutar este comando después de ejecutar [az login](/cli/azure/reference-index#az-login) para iniciar sesión.



## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Selección del destino de la versión 2.0 del runtime en el entorno de desarrollo local](functions-run-local.md)

> [!div class="nextstepaction"]
> [Vea las notas de versión para las versiones del runtime](https://github.com/Azure/azure-webjobs-sdk-script/releases)
