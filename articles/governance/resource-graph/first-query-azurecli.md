---
title: Ejecución de la primera consulta con la CLI de Azure
description: Este artículo le guiará por los pasos para habilitar la extensión Resource Graph para la CLI de Azure y ejecutar la primera consulta.
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/18/2019
ms.topic: quickstart
ms.service: resource-graph
ms.openlocfilehash: 0136b101de870ce669e89e67679719e233343330
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73622581"
---
# <a name="quickstart-run-your-first-resource-graph-query-using-azure-cli"></a>Inicio rápido: Ejecución de la primera consulta de Resource Graph con la CLI de Azure

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

1. Compruebe que la extensión se ha instalado y que es la versión esperada (al menos **1.0.0**):

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
   az graph query -q 'Resources | project name, type | limit 5'
   ```

   > [!NOTE]
   > Como esta consulta de ejemplo no proporciona un modificador de ordenación como `order by`, es probable que al ejecutar esta consulta varias veces se produzca un conjunto diferente de recursos por solicitud.

1. Actualice la consulta para `order by` la propiedad **Nombre**:

   ```azurecli-interactive
   # Run Azure Resource Graph query with 'order by'
   az graph query -q 'Resources | project name, type | limit 5 | order by name asc'
   ```

   > [!NOTE]
   > Al igual que con la primera consulta, es probable que al ejecutar esta consulta varias veces se produzca un conjunto diferente de recursos por solicitud. El orden de los comandos de consulta es importante. En este ejemplo, el `order by` viene después del `limit`. Esto primero limitará los resultados de la consulta y después los ordenará.

1. Actualice la consulta para que primero se aplique `order by` a la propiedad **Nombre** y, después, `limit` a los cinco primeros resultados:

   ```azurecli-interactive
   # Run Azure Resource Graph query with `order by` first, then with `limit`
   az graph query -q 'Resources | project name, type | order by name asc | limit 5'
   ```

Cuando la consulta final se ejecuta varias veces, suponiendo que nada en su entorno esté cambiando, los resultados devueltos serán coherentes y como se esperaban, ordenados por la propiedad **Nombre**, pero todavía limitados a los cinco primeros resultados.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere quitar la extensión de Resource Graph de su entorno de la CLI de Azure, puede hacerlo mediante el comando siguiente:

```azurecli-interactive
# Remove the Resource Graph extension from the Azure CLI environment
az extension remove -n resource-graph
```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre el [lenguaje de consulta](./concepts/query-language.md).
- Obtenga más información sobre cómo [explorar recursos](./concepts/explore-resources.md).
- Ejecute la primera consulta mediante [Azure Portal](first-query-portal.md).
- Ejecute la primera consulta con [Azure PowerShell](first-query-powershell.md).
- Consulte ejemplos de [consultas de inicio](./samples/starter.md).
- Vea ejemplos de [consultas avanzadas](./samples/advanced.md).
- Proporcione comentarios sobre [UserVoice](https://feedback.azure.com/forums/915958-azure-governance).