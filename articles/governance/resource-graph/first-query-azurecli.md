---
title: Ejecución de la primera consulta con la CLI de Azure
description: Este artículo le guiará por los pasos para habilitar la extensión Resource Graph para la CLI de Azure y ejecutar la primera consulta.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/22/2018
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 8f5d98ff591cb456cbbcb3d28a63f39ab3729152
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53308522"
---
# <a name="run-your-first-resource-graph-query-using-azure-cli"></a>Ejecución de la primera consulta de Resource Graph con la CLI de Azure

El primer paso para usar Azure Resource Graph es comprobar que está instalada la extensión para la [CLI de Azure](/cli/azure/). Esta guía de inicio rápido lo guiará a través del proceso de agregar la extensión a la instalación de la CLI de Azure. Puede usar la extensión con la CLI de Azure instalada localmente o mediante [Azure Cloud Shell](https://shell.azure.com).

Al final de este proceso, habrá agregado la extensión a la instalación de la CLI de Azure que haya elegido y habrá ejecutado la primera consulta de Resource Graph.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="add-the-resource-graph-extension"></a>Incorporación de la extensión de Resource Graph

Para habilitar la CLI de Azure para consultar Azure Resource Graph, se debe agregar la extensión. Esta extensión funciona siempre que se puede usar la CLI de Azure, incluidos [Bash en Windows 10](/windows/wsl/install-win10), [Cloud Shell](https://shell.azure.com) (independiente y dentro del portal), la [imagen de Docker de la CLI de Azure](https://hub.docker.com/r/microsoft/azure-cli/) o instalado localmente.

1. Compruebe que está instalada la versión más reciente de la CLI de Azure (al menos la **2.0.45**). Si todavía no está instalado, siga [estas instrucciones](/cli/azure/install-azure-cli-windows?view=azure-cli-latest).

1. En el entorno que prefiera de la CLI de Azure, impórtela con el siguiente comando:

   ```azurecli-interactive
   # Add the Resource Graph extension to the Azure CLI environment
   az extension add --name resource-graph
   ```

1. Compruebe que la extensión se ha instalado y que es la versión esperada (al menos la **0.1.7**):

   ```azurecli-interactive
   # Check the extension list (note that you may have other extensions installed)
   az extension list

   # Run help for graph query options
   az graph query -h
   ```

## <a name="run-your-first-resource-graph-query"></a>Ejecutar la primera consulta de Resource Graph

Una vez agregada la extensión de la CLI de Azure al entorno de su elección, es el momento de probar una consulta simple de Resource Graph. La consulta devolverá los cinco primeros recursos de Azure con el **Nombre** y el **Tipo de recurso** de cada recurso.

1. Ejecute la primera consulta de Azure Resource Graph mediante la extensión `graph` y el comando `query`:

   ```azurecli-interactive
   # Login first with az login if not using Cloud Shell

   # Run Azure Resource Graph query
   az graph query -q 'project name, type | limit 5'
   ```

   > [!NOTE]
   > Como esta consulta de ejemplo no proporciona un modificador de ordenación como `order by`, es probable que al ejecutar esta consulta varias veces se produzca un conjunto diferente de recursos por solicitud.

1. Actualice la consulta para `order by` la propiedad **Nombre**:

   ```azurecli-interactive
   # Run Azure Resource Graph query with 'order by'
   az graph query -q 'project name, type | limit 5 | order by name asc'
   ```

  > [!NOTE]
  > Al igual que con la primera consulta, es probable que al ejecutar esta consulta varias veces se produzca un conjunto diferente de recursos por solicitud. El orden de los comandos de consulta es importante. En este ejemplo, el `order by` viene después del `limit`. Esto primero limitará los resultados de la consulta y después los ordenará.

1. Actualice la consulta para que primero se aplique `order by` a la propiedad **Nombre** y, después, `limit` a los cinco primeros resultados:

   ```azurecli-interactive
   # Run Azure Resource Graph query with `order by` first, then with `limit`
   az graph query -q 'project name, type | order by name asc | limit 5'
   ```

Cuando la consulta final se ejecuta varias veces, suponiendo que nada en su entorno esté cambiando, los resultados devueltos serán coherentes y como se esperaban, ordenados por la propiedad **Nombre**, pero todavía limitados a los cinco primeros resultados.

## <a name="cleanup"></a>Limpieza

Si quiere quitar la extensión de Resource Graph de su entorno de la CLI de Azure, puede hacerlo mediante el comando siguiente:

```azurecli-interactive
# Remove the Resource Graph extension from the Azure CLI environment
az extension remove -n resource-graph
```

> [!NOTE]
> Esto no elimina el archivo de la extensión descargado anteriormente. Solo lo quita del entorno de la CLI de Azure en ejecución.

## <a name="next-steps"></a>Pasos siguientes

- Obtener más información sobre el [lenguaje de consulta](./concepts/query-language.md)
- Más información sobre la [exploración de recursos](./concepts/explore-resources.md)
- Ejecución de la primera consulta con [Azure PowerShell](first-query-powershell.md)
- Consulte ejemplos de [consultas de inicio](./samples/starter.md)
- Ver ejemplos de [consultas de nivel avanzado](./samples/advanced.md)
- Comentarios sobre [UserVoice](https://feedback.azure.com/forums/915958-azure-governance)