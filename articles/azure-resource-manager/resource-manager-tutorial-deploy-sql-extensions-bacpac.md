---
title: Importación de archivos BACPAC de SQL con plantillas de Azure Resource Manager | Microsoft Docs
description: Aprenda a usar la extensión de SQL Database para importar archivos BACPAC de SQL con plantillas de Azure Resource Manager.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 02/25/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 6c146a99be369f92a72717ad1ec4b79bddafe28c
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56819333"
---
# <a name="tutorial-import-sql-bacpac-files-with-azure-resource-manager-templates"></a>Tutorial: Importación de archivos BACPAC de SQL con plantillas de Azure Resource Manager

Aprenda a usar las extensiones de Azure SQL Database para importar un archivo BACPAC con las plantillas de Azure Resource Manager. Los artefactos de implementación son cualquier archivo, además del archivo de plantilla principal, que se necesitan para completar una implementación. El archivo BACPAC es un artefacto. En este tutorial, creará una plantilla para implementar una instancia de Azure SQL Server, una instancia de SQL Database e importar un archivo BACPAC. Para información sobre la implementación de extensiones de máquina virtual de Azure mediante plantillas de Azure Resource Manager, consulte [# Tutorial: Implementación de extensiones de máquina virtual con plantillas de Azure Resource Manager](./resource-manager-tutorial-deploy-vm-extensions.md).

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

* [Visual Studio Code](https://code.visualstudio.com/) con la extensión Resource Manager Tools. Consulte [Instalación de la extensión ](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Para aumentar la seguridad, utilice una contraseña generada para la cuenta de administrador de SQL Server. Este es un ejemplo para generar una contraseña:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault está diseñado para proteger las claves criptográficas y otros secretos. Para más información, consulte [Tutorial: Integración de Azure Key Vault en Resource Manager Template Deployment](./resource-manager-tutorial-use-key-vault.md). También se recomienda actualizar la contraseña cada tres meses.

## <a name="prepare-a-bacpac-file"></a>Preparación de un archivo BACPAC

Un archivo BACPAC se comparte en una [cuenta de Azure Storage](https://armtutorials.blob.core.windows.net/sqlextensionbacpac/SQLDatabaseExtension.bacpac) con el acceso público. Para crear el suyo propio, consulte [Exportación de una base de datos de Azure SQL Database a un archivo BACPAC](../sql-database/sql-database-export.md) Si elige publicar el archivo en su propia ubicación, tendrá que actualizar la plantilla más adelante en el tutorial.

## <a name="open-a-quickstart-template"></a>Abra una plantilla de inicio rápido.

Las plantillas de inicio rápido de Azure consisten en un repositorio de plantillas de Resource Manager. En lugar de crear una plantilla desde cero, puede buscar una plantilla de ejemplo y personalizarla. La plantilla que se usa en este tutorial se denomina [Deploy an Azure SQL Server with Threat Detection](https://azure.microsoft.com/resources/templates/201-sql-threat-detection-server-policy-optional-db/) (Implementación de una instancia de Azure SQL Server con detección de amenazas).

1. En Visual Studio Code, seleccione **Archivo**>**Abrir archivo**.
2. En **Nombre de archivo**, pegue el código URL siguiente:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-sql-threat-detection-server-policy-optional-db/azuredeploy.json
    ```
3. Seleccione **Abrir** para abrir el archivo.

    En la plantilla hay tres recursos definidos.

    * `Microsoft.Sql/servers`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.sql/servers).
    * `Microsoft.SQL/servers/securityAlertPolicies`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/securityalertpolicies).
    * `Microsoft.SQL.servers/databases`.  Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).

    Resulta útil obtener cierta información básica de la plantilla antes de personalizarla.
4. Seleccione **Archivo**>**Guardar como** para guardar una copia del archivo en la máquina local con el nombre **azuredeploy.json**.

## <a name="edit-the-template"></a>Edición de la plantilla

Agregue dos recursos adicionales a la plantilla.

* Para permitir que la extensión de la base de datos SQL importe archivos BACPAC, necesita permitir el acceso a los servicios de Azure. Agregue el siguiente código JSON a la definición de servidor SQL:

    ```json
    {
        "type": "firewallrules",
        "name": "AllowAllAzureIps",
        "location": "[parameters('location')]",
        "apiVersion": "2015-05-01-preview",
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
            "name": "Import",
            "type": "extensions",
            "apiVersion": "2014-04-01",
            "dependsOn": [
                "[resourceId('Microsoft.Sql/servers/databases', variables('databaseServerName'), variables('databaseName'))]"
            ],
            "properties": {
                "storageKeyType": "SharedAccessKey",
                "storageKey": "?",
                "storageUri": "https://armtutorials.blob.core.windows.net/sqlextensionbacpac/SQLDatabaseExtension.bacpac",
                "administratorLogin": "[variables('databaseServerAdminLogin')]",
                "administratorLoginPassword": "[variables('databaseServerAdminLoginPassword')]",
                "operationMode": "Import",
            }
        }
    ]
    ```

    La plantilla será así:

    ![Azure Resource Manager implementa extensiones de sql BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac.png)

    Para conocer la definición de recursos, consulte la [referencia de extensiones de SQL Database](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases/extensions). Estos son algunos elementos importantes:

    * **dependsOn**: el recurso de la extensión debe crearse después de que se haya creado la base de datos SQL.
    * **storageKeyType**: el tipo de clave de almacenamiento que se usa. El valor puede ser `StorageAccessKey` o `SharedAccessKey`. Dado que el archivo BACPAC proporcionado se comparte en una cuenta de Azure Storage con acceso público, aquí se usa "SharedAccessKey".
    * **storageKey**: la clave de almacenamiento que se usa. Si el tipo de clave de almacenamiento es SharedAccessKey, deben ir precedida de un "?."
    * **storageUri**: el URI de almacenamiento que se usa. Si elige no utilizar el archivo BACPAC que se proporciona, deberá actualizar los valores.
    * **administratorLoginPassword**: la contraseña del administrador SQL. Use una contraseña generada. Consulte [Requisitos previos](#prerequisites).

## <a name="deploy-the-template"></a>Implementación de la plantilla

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Consulte la sección [Implementación de la plantilla](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) para conocer el procedimiento de implementación. En su lugar, use el siguiente script de implementación de PowerShell.

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the SQL admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -TemplateFile "$HOME/azuredeploy.json"
```

Use una contraseña generada. Consulte [Requisitos previos](#prerequisites).

## <a name="verify-the-deployment"></a>Comprobar la implementación

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
