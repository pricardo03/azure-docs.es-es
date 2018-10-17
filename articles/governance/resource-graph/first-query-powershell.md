---
title: Ejecutar la primera consulta de Resource Graph con Azure PowerShell
description: Este artículo le guiará por los pasos para habilitar el módulo de Resource Graph de Azure PowerShell y ejecutar la primera consulta.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: resource-graph
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 1a2bc5626e94f5fcb0ec8c2be8d91c8fc6484e0b
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224569"
---
# <a name="run-your-first-resource-graph-query-using-azure-powershell"></a>Ejecutar la primera consulta de Resource Graph con Azure PowerShell

El primer paso para usar Azure Resource Graph es asegurarse de que está instalado el módulo de Azure PowerShell. Esta guía de inicio rápido lo guiará a través del proceso de agregar el módulo a la instalación de Azure PowerShell.

Al final de este proceso, habrá agregado el módulo a la instalación de Azure PowerShell que haya elegido y habrá ejecutado la primera consulta de Resource Graph.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="add-the-resource-graph-module"></a>Agregar el módulo de Resource Graph

Para habilitar Azure PowerShell para consultar Azure Resource Graph, se debe agregar el módulo. Este módulo se puede usar con Windows PowerShell y PowerShell Core instalados localmente, así como la [imagen de Docker de Azure PowerShell](https://hub.docker.com/r/azuresdk/azure-powershell/).

### <a name="base-requirements"></a>Requisitos básicos

El módulo de Azure Resource Graph requiere el software siguiente:

- Azure PowerShell 6.3.0 o posterior. Si todavía no está instalado, siga [estas instrucciones](/powershell/azure/install-azurerm-ps).

  - Para PowerShell Core, use la versión **Az** del módulo de Azure PowerShell.

  - Para Windows PowerShell, use la versión **AzureRm** del módulo de Azure PowerShell.

  > [!NOTE]
  > Actualmente no se recomienda instalar el módulo en Cloud Shell.

- PowerShellGet. Si no está instalado o actualizado, siga [estas instrucciones](/powershell/gallery/installing-psget).

### <a name="powershell-core"></a>PowerShell Core

El módulo de Resource Graph para PowerShell Core es **Az.ResourceGraph**.

1. Ejecute el siguiente comando como **administrador** en un símbolo del sistema de PowerShell Core:

   ```powershell
   # Install the Resource Graph module from PowerShell Gallery
   Install-Module -Name Az.ResourceGraph
   ```

1. Valide que el módulo se ha importado y que sea la versión correcta (0.2.0):

   ```powershell
   # Get a list of commands for the imported Az.ResourceGraph module
   Get-Command -Module 'Az.ResourceGraph' -CommandType 'Cmdlet'
   ```

1. Habilite los alias de versiones anteriores para **Az** a **AzureRm** con el siguiente comando:

   ```powershell
   # Enable backwards alias compatibility
   Enable-AzureRmAlias
   ```

### <a name="windows-powershell"></a>Windows PowerShell

El módulo de Resource Graph para Windows PowerShell es **AzureRm.ResourceGraph**.

1. Ejecute el siguiente comando como **administrador** en un símbolo del sistema de Windows PowerShell:

   ```powershell
   # Install the Resource Graph (prerelease) module from PowerShell Gallery
   Install-Module -Name AzureRm.ResourceGraph -AllowPrerelease
   ```

1. Valide que el módulo se ha importado y que sea la versión correcta (0.1.0-preview):

   ```powershell
   # Get a list of commands for the imported AzureRm.ResourceGraph module
   Get-Command -Module 'AzureRm.ResourceGraph' -CommandType 'Cmdlet'
   ```

## <a name="run-your-first-resource-graph-query"></a>Ejecutar la primera consulta de Resource Graph

Ahora que el módulo de Azure PowerShell se ha agregado al entorno de su elección, es el momento de probar una consulta simple de Resource Graph. La consulta devolverá los cinco primeros recursos de Azure con el **Nombre** y el **Tipo de recurso** de cada recurso.

1. Ejecute la primera consulta de Azure Resource Graph mediante el cmdlet `Search-AzureRmGraph`:

   ```powershell
   # Login first with Connect-AzureRmAccount

   # Run Azure Resource Graph query
   Search-AzureRmGraph -Query 'project name, type | limit 5'
   ```

   > [!NOTE]
   > Como esta consulta de ejemplo no proporciona un modificador de ordenación como `order by`, es probable que al ejecutar esta consulta varias veces se produzca un conjunto diferente de recursos por solicitud.

1. Actualice la consulta para `order by` la propiedad **Nombre**:

   ```powershell
   # Run Azure Resource Graph query with 'order by'
   Search-AzureRmGraph -Query 'project name, type | limit 5 | order by name asc'
   ```

  > [!NOTE]
  > Al igual que con la primera consulta, es probable que al ejecutar esta consulta varias veces se produzca un conjunto diferente de recursos por solicitud. El orden de los comandos de consulta es importante. En este ejemplo, el `order by` viene después del `limit`. Esto primero limitará los resultados de la consulta y después los ordenará.

1. Actualice la consulta para que primero `order by` la propiedad **Nombre** y, después, `limit` a los cinco primeros resultados:

   ```powershell
   # Run Azure Resource Graph query with `order by` first, then with `limit`
   Search-AzureRmGraph -Query 'project name, type | order by name asc | limit 5'
   ```

Cuando la consulta final se ejecuta varias veces, suponiendo que nada en su entorno esté cambiando, los resultados devueltos serán coherentes y como se esperaban, ordenados por la propiedad **Nombre**, pero todavía limitados a los cinco primeros resultados.

## <a name="cleanup"></a>Limpieza

Si quiere quitar el módulo de Resource Graph de su entorno de Azure PowerShell, puede hacerlo mediante el comando siguiente:

```powershell
# Remove the Resource Graph module from the Azure PowerShell environment
Remove-Module -Name 'AzureRm.ResourceGraph'
```

> [!NOTE]
> Esto no elimina el archivo de módulo descargado anteriormente. Solo lo quita de la sesión de PowerShell en ejecución.

## <a name="next-steps"></a>Pasos siguientes

- Obtener más información sobre el [lenguaje de consulta](./concepts/query-language.md)
- Obtener información sobre cómo [explorar recursos](./concepts/explore-resources.md)
- Ejecución de la primera consulta con la [CLI de Azure](first-query-azurecli.md)
- Ver ejemplos de [consultas de nivel principiante](./samples/starter.md)
- Ver ejemplos de [consultas de nivel avanzado](./samples/advanced.md)
- Comentarios sobre [UserVoice](https://feedback.azure.com/forums/915958-azure-governance)