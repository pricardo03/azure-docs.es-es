---
title: Importación de archivos BACPAC de SQL con plantillas
description: Aprenda a usar la extensión de SQL Database para importar archivos BACPAC de SQL con plantillas de Azure Resource Manager.
author: mumian
ms.date: 11/21/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 741521551335712400e5f61822d7dda31199d3df
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2019
ms.locfileid: "74422173"
---
# <a name="tutorial-import-sql-bacpac-files-with-azure-resource-manager-templates"></a>Tutorial: Importación de archivos BACPAC de SQL con plantillas de Azure Resource Manager

Aprenda a usar las extensiones de Azure SQL Database para importar un archivo BACPAC con las plantillas de Azure Resource Manager. Los artefactos de implementación son cualquier archivo, además de los archivos de la plantilla principal, que se necesitan para completar una implementación. El archivo BACPAC es un artefacto. En este tutorial, creará una plantilla para implementar una instancia de Azure SQL Server, una instancia de SQL Database e importar un archivo BACPAC. Para información sobre la implementación de extensiones de máquina virtual de Azure mediante plantillas de Azure Resource Manager, consulte [# Tutorial: Implementación de extensiones de máquina virtual con plantillas de Azure Resource Manager](./resource-manager-tutorial-deploy-vm-extensions.md).

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Preparación de un archivo BACPAC
> * Abra una plantilla de inicio rápido.
> * Edición de la plantilla
> * Implementación de la plantilla
> * Comprobar la implementación

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesitará lo siguiente:

* Visual Studio Code con la extensión Resource Manager Tools. Consulte [Uso de Visual Studio Code para crear plantillas de Azure Resource Manager](./resource-manager-tools-vs-code.md).
* Para aumentar la seguridad, utilice una contraseña generada para la cuenta de administrador de SQL Server. Este es un ejemplo para generar una contraseña:

    ```azurecli-interactive
    openssl rand -base64 32
    ```

    Azure Key Vault está diseñado para proteger las claves criptográficas y otros secretos. Para más información, consulte [Tutorial: Integración de Azure Key Vault en Resource Manager Template Deployment](./resource-manager-tutorial-use-key-vault.md). También se recomienda actualizar la contraseña cada tres meses.

## <a name="prepare-a-bacpac-file"></a>Preparación de un archivo BACPAC

En [GitHub](https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-sql-extension/SQLDatabaseExtension.bacpac) se comparte un archivo BACPAC. Para crear el suyo propio, consulte [Exportación de una base de datos de Azure SQL Database a un archivo BACPAC](../sql-database/sql-database-export.md) Si elige publicar el archivo en su propia ubicación, tendrá que actualizar la plantilla más adelante en el tutorial.

El archivo BACPAC se debe almacenar en una cuenta de Azure Storage antes de que se pueda importar mediante una plantilla de Resource Manager.

1. Abra [Cloud Shell](https://shell.azure.com).
1. Seleccione **Cargar/Descargar archivos** y, después, seleccione **Cargar**.
1. Especifique la siguiente dirección URL y, a continuación, seleccione **Abrir**.

    ```url
    https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-sql-extension/SQLDatabaseExtension.bacpac
    ```

1. Copie y pegue el siguiente script de PowerShell en la ventana de Cloud Shell.

    ```azurepowershell-interactive
    $projectName = Read-Host -Prompt "Enter a project name that is used to generate Azure resource names"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"

    $resourceGroupName = "${projectName}rg"
    $storageAccountName = "${projectName}store"
    $containerName = "bacpacfiles"
    $bacpacFile = "$HOME/SQLDatabaseExtension.bacpac"
    $blobName = "SQLDatabaseExtension.bacpac"

    New-AzResourceGroup -Name $resourceGroupName -Location $location
    $storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroupName `
                                           -Name $storageAccountName `
                                           -SkuName Standard_LRS `
                                           -Location $location
    $storageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName `
                                                  -Name $storageAccountName).Value[0]

    New-AzStorageContainer -Name $containerName -Context $storageAccount.Context

    Set-AzStorageBlobContent -File $bacpacFile `
                             -Container $containerName `
                             -Blob $blobName `
                             -Context $storageAccount.Context

    Write-Host "The storage account key is $storageAccountKey"
    Write-Host "The BACPAC file URL is https://$storageAccountName.blob.core.windows.net/$containerName/$blobName"
    Write-Host "Press [ENTER] to continue ..."
    ```

1. Anote la clave de la cuenta de almacenamiento y la dirección URL del archivo BACPAC. Necesitará estos valores al implementar la plantilla.

## <a name="open-a-quickstart-template"></a>Abra una plantilla de inicio rápido.

La plantilla que se usa en este tutorial se almacena en [GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json).

1. En Visual Studio Code, seleccione **Archivo**>**Abrir archivo**.
2. En **Nombre de archivo**, pegue el código URL siguiente:

    ```url
    https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json
    ```

3. Seleccione **Abrir** para abrir el archivo.

    En la plantilla hay tres recursos definidos.

   * `Microsoft.Sql/servers`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.sql/servers).
   * `Microsoft.SQL/servers/securityAlertPolicies`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/securityalertpolicies).
   * `Microsoft.SQL.servers/databases`.  Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).

     Resulta útil obtener cierta información básica de la plantilla antes de personalizarla.
4. Seleccione **Archivo**>**Guardar como** para guardar una copia del archivo en la máquina local con el nombre **azuredeploy.json**.

## <a name="edit-the-template"></a>Edición de la plantilla

1. Agregue dos parámetros más al final de la sección **parameters** para establecer la clave de la cuenta de almacenamiento y la dirección URL de BACPAC:

    ```json
    "storageAccountKey": {
      "type":"string",
      "metadata":{
        "description": "Specifies the key of the storage account where the BACPAC file is stored."
      }
    },
    "bacpacUrl": {
      "type":"string",
      "metadata":{
        "description": "Specifies the URL of the BACPAC file."
      }
    }
    ```

    Agregue una coma después de **adminPassword**. Para dar formato al archivo JSON desde VS Code, presione **[SHIFT]+[ALT]+F**.

    Consulte [Preparación de un archivo BACPAC](#prepare-a-bacpac-file) sobre cómo obtener estos dos valores.

1. Agregue dos recursos adicionales a la plantilla.

    * Para permitir que la extensión de la base de datos SQL importe archivos BACPAC, necesita permitir el tráfico desde los servicios de Azure. Agregue la siguiente definición de regla de firewall en la definición de SQL Server:

        ```json
        {
          "type": "firewallrules",
          "apiVersion": "2015-05-01-preview",
          "name": "AllowAllAzureIps",
          "location": "[parameters('location')]",
          "dependsOn": [
            "[variables('databaseServerName')]"
          ],
          "properties": {
            "startIpAddress": "0.0.0.0",
            "endIpAddress": "0.0.0.0"
          }
        }
        ```

        La plantilla será así:

        ![Azure Resource Manager implementa extensiones de sql BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-firewall.png)

    * Agregue un recurso de extensión de SQL Database a la definición de la base de datos con el siguiente código JSON:

        ```json
        "resources": [
            {
              "type": "extensions",
              "apiVersion": "2014-04-01",
              "name": "Import",
              "dependsOn": [
                "[resourceId('Microsoft.Sql/servers/databases', variables('databaseServerName'), variables('databaseName'))]"
              ],
              "properties": {
                "storageKeyType": "StorageAccessKey",
                "storageKey": "[parameters('storageAccountKey')]",
                "storageUri": "[parameters('bacpacUrl')]",
                "administratorLogin": "[variables('databaseServerAdminLogin')]",
                "administratorLoginPassword": "[variables('databaseServerAdminLoginPassword')]",
                "operationMode": "Import"
              }
            }
        ]
        ```

        La plantilla será así:

        ![Azure Resource Manager implementa extensiones de sql BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac.png)

        Para conocer la definición de recursos, consulte la [referencia de extensiones de SQL Database](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases/extensions). Estos son algunos elementos importantes:

        * **dependsOn**: el recurso de la extensión debe crearse después de que se haya creado la base de datos SQL.
        * **storageKeyType**: especifique el tipo de clave de almacenamiento que se usa. El valor puede ser `StorageAccessKey` o `SharedAccessKey`. Use `StorageAccessKey` en este tutorial.
        * **storageKey**: Especifique la clave de la cuenta de almacenamiento donde se almacena el archivo BACPAC. Si el tipo de clave de almacenamiento es SharedAccessKey, deben ir precedida de un "?".
        * **storageUri**: Especifique la dirección URL del archivo BACPAC almacenado en una cuenta de almacenamiento.
        * **administratorLoginPassword**: la contraseña del administrador SQL. Use una contraseña generada. Consulte [Requisitos previos](#prerequisites).

## <a name="deploy-the-template"></a>Implementación de la plantilla

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Consulte la sección [Implementación de la plantilla](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) para conocer el procedimiento de implementación. En su lugar, use el siguiente script de implementación de PowerShell.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used to generate Azure resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the SQL admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
$storageAccountKey = Read-Host -Prompt "Enter the storage account key"
$bacpacUrl = Read-Host -Prompt "Enter the URL of the BACPAC file"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -TemplateFile "$HOME/azuredeploy.json" `
    -storageAccountKey $storageAccountKey `
    -bacpacUrl $bacpacUrl

Write-Host "Press [ENTER] to continue ..."
```

Considere la posibilidad de usar el mismo nombre de proyecto que usó al preparar el archivo BacPac, de modo que todos los recursos se almacenen en el mismo grupo de recursos.  Facilita la administración de los recursos; por ejemplo, la limpieza de recursos. Si usa el mismo nombre de proyecto, puede eliminar el comando **New-AzResourceGroup** del script o bien responder Sí o No cuando se le pregunte si desea actualizar el grupo de recursos existente.

Use una contraseña generada. Consulte [Requisitos previos](#prerequisites).

## <a name="verify-the-deployment"></a>Comprobar la implementación

Para tener acceso a SQL Server desde el equipo cliente, debe agregar una regla de firewall adicional. Para más información, consulte [Creación y administración de reglas de firewall de IP](../sql-database/sql-database-firewall-configure.md#create-and-manage-ip-firewall-rules).

En el portal, seleccione la base de datos SQL del grupo de recursos recién implementado. Seleccione **Editor de consultas (versión preliminar)** y, después, escriba las credenciales de administrador. Verá dos tablas importadas en la base de datos:

![Azure Resource Manager implementa extensiones de sql BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-query-editor.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
2. Escriba el nombre del grupo de recursos en el campo **Filtrar por nombre**.
3. Seleccione el nombre del grupo de recursos.  Verá un total de seis recursos en el grupo de recursos.
4. Seleccione **Eliminar grupo de recursos** del menú superior.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha implementado una instancia de SQL Server y una instancia de SQL Database, y ha importado un archivo BACPAC. El archivo BACPAC se almacena en una cuenta de almacenamiento de Azure. Cualquier persona con la dirección URL puede acceder al archivo. Para obtener información sobre cómo proteger el archivo BACPAC (artefacto), consulte

> [!div class="nextstepaction"]
> [Proteger los artefactos](./resource-manager-tutorial-secure-artifacts.md)
