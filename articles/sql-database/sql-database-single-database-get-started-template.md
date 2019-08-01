---
title: 'Azure Resource Manager: Creación de una base de datos única en Azure SQL Database | Microsoft Docs'
description: Cree una base de datos única en Azure SQL Database con una plantilla de Azure Resource Manager.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: mumian
ms.author: jgao
ms.reviewer: carlrab
ms.date: 06/28/2019
ms.openlocfilehash: f3e9bb0e9a2c4c58a205798441ddc2208019e7d2
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68566575"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-resource-manager-template"></a>Inicio rápido: Creación de una base de datos única en Azure SQL Database con una plantilla de Azure Resource Manager

La creación de una [base de datos única](sql-database-single-database.md) es la opción de implementación más rápida y sencilla para crear una base de datos en Azure SQL Database. En este inicio rápido se muestra cómo crear una base de datos única con una plantilla de Azure Resource Manager. Para más información, consulte la [documentación de Azure Resource Manager](/azure/azure-resource-manager/).

Si no tiene una suscripción a Azure, [cree una cuenta gratuita](https://azure.microsoft.com/free/).

## <a name="create-a-single-database"></a>Creación de una base de datos única

Una base de datos única tiene un conjunto definido de recursos de proceso, memoria, E/S y almacenamiento y se usa uno de los dos [modelos de compra](sql-database-purchase-models.md). Cuando se crea una base de datos única, también se define un [servidor de SQL Database](sql-database-servers.md) para administrarla y colocarla dentro de un [grupo de recursos de Azure](../azure-resource-manager/resource-group-overview.md) de una región determinada.

El siguiente archivo JSON es la plantilla que se usa en este artículo. La plantilla se almacena en [GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/SQLServerAndDatabase/azuredeploy.json). Puede encontrar más ejemplos de plantillas de la base de datos de Azure SQL en [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Sql&pageNumber=1&sort=Popular).

[!code-json[create-azure-sql-database-server-and-database](~/resourcemanager-templates/SQLServerAndDatabase/azuredeploy.json)]

1. Seleccione **Pruébelo** en el bloque de código de PowerShell siguiente para abrir Azure Cloud Shell.

    ```azurepowershell-interactive
    $projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
    $location = Read-Host -Prompt "Enter an Azure location (i.e. centralus)"
    $adminUser = Read-Host -Prompt "Enter the SQL server administrator username"
    $adminPassword = Read-Host -Prompt "Enter the SQl server administrator password" -AsSecureString

    $resourceGroupName = "${projectName}rg"


    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile "D:\GitHub\azure-docs-json-samples\SQLServerAndDatabase\azuredeploy.json" -projectName $projectName -adminUser $adminUser -adminPassword $adminPassword

    Read-Host -Prompt "Press [ENTER] to continue ..."
    ```

1. Seleccione **Copiar** para copiar el script de PowerShell en el Portapapeles.
1. Haga clic con el botón derecho en el panel del shell y, a continuación, seleccione **Pegar**.

    Se tardan unos minutos en crear el servidor de base de datos y la base de datos.

## <a name="query-the-database"></a>Consulta de la base de datos

Para consultar la base de datos, vea [Consulta de la base de datos](./sql-database-single-database-get-started.md#query-the-database).

## <a name="clean-up-resources"></a>Limpieza de recursos

Mantenga este grupo de recursos, el servidor de bases de datos y la base de datos única si quiere ir a [Pasos siguientes](#next-steps). Los pasos siguientes muestran cómo conectarse a la base de datos y consultarla mediante distintos métodos.

Para eliminar el grupo de recursos mediante Azure PowerShell:

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName

Read-Host -Prompt "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Pasos siguientes

- Cree una regla de firewall de nivel de servidor para conectarse a la base de datos única desde herramientas locales o remotas. Para más información, consulte [Creación de una regla de firewall de nivel de servidor](sql-database-server-level-firewall-rule.md).
- Después de crear una regla de firewall de nivel de servidor, puede usar varias herramientas y lenguajes para [conectarse a la base de datos y realizar consultas en ella](sql-database-connect-query.md).
  - [Conexión y consulta con SQL Server Management Studio](sql-database-connect-query-ssms.md)
  - [Conexión y consulta de datos con Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Para crear una base de datos única mediante la CLI de Azure, consulte [Ejemplos de la CLI de Azure](sql-database-cli-samples.md).
- Para crear una base de datos única mediante Azure PowerShell, consulte [Ejemplos de Azure PowerShell](sql-database-powershell-samples.md).
