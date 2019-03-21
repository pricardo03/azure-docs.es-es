---
title: Protección de los artefactos de las implementaciones de plantilla de Azure Resource Manager | Microsoft Docs
description: Obtenga información sobre cómo proteger los artefactos que se usan en las plantillas de Azure Resource Manager.
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
ms.openlocfilehash: f7f235ce709fd81c4bb4c367774b4a96cd920e13
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58120353"
---
# <a name="tutorial-secure-artifacts-in-azure-resource-manager-template-deployments"></a>Tutorial: Protección de los artefactos de las implementaciones de plantillas de Azure Resource Manager

Obtenga información sobre cómo proteger los artefactos que se usan en las plantillas de Azure Resource Manager mediante la cuenta de Azure Storage con firmas de acceso compartido (SAS). Los artefactos de implementación son cualquier archivo, además del archivo de plantilla principal, que se necesitan para completar una implementación. Por ejemplo, en [Tutorial: Importación de archivos BACPAC de SQL con plantillas de Azure Resource Manager](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md), la plantilla principal crea una base de datos SQL de Azure; también llama a un archivo BACPAC para crear tablas e insertar datos. El archivo BACPAC es un artefacto. El artefacto se almacena en una cuenta de Azure Storage con acceso público. En este tutorial, usará SAS para conceder acceso limitado al archivo BACPAC en su propia cuenta de Azure Storage. Para obtener más información sobre SAS, consulte [Uso de firmas de acceso compartido (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

Si quiere más detalles sobre cómo proteger plantillas vinculadas, consulte [Tutorial: Creación de plantillas vinculadas de Azure Resource Manager](./resource-manager-tutorial-create-linked-templates.md).

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Preparación de un archivo BACPAC
> * Apertura de una plantilla existente
> * Edición de la plantilla
> * Implementación de la plantilla
> * Comprobar la implementación

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesitará lo siguiente:

* [Visual Studio Code](https://code.visualstudio.com/) con la extensión Resource Manager Tools. Consulte [Instalación de la extensión ](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Revise [Tutorial: Importación de archivos BACPAC de SQL con plantillas de Azure Resource Manager](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md). La plantilla usada en este tutorial es lo que se desarrolló en ese mismo tutorial. En este artículo se proporciona un vínculo de descarga de la plantilla completa.
* Para aumentar la seguridad, utilice una contraseña generada para la cuenta de administrador de SQL Server. Este es un ejemplo para generar una contraseña:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault está diseñado para proteger las claves criptográficas y otros secretos. Para más información, consulte [Tutorial: Integración de Azure Key Vault en Resource Manager Template Deployment](./resource-manager-tutorial-use-key-vault.md). También se recomienda actualizar la contraseña cada tres meses.

## <a name="prepare-a-bacpac-file"></a>Preparación de un archivo BACPAC

En esta sección, preparará el archivo BACPAC para que se pueda acceder a él de forma segura al implementar la plantilla de Resource Manager. Esta sección tiene cinco procedimientos:

* Descargue el archivo BACPAC.
* Cree una cuenta de Azure Storage.
* Cree un contenedor de blobs en una cuenta de almacenamiento.
* Cargue el archivo BACPAC en el contenedor.
* Recupere el token de SAS del archivo BACPAC.

Para automatizar estos pasos con un script de PowerShell, consulte el script de [Carga de la plantilla vinculada](./resource-manager-tutorial-create-linked-templates.md#upload-the-linked-template).

### <a name="download-the-bacpac-file"></a>Descarga del archivo BACPAC

Descargue el [archivo BACPAC](https://armtutorials.blob.core.windows.net/sqlextensionbacpac/SQLDatabaseExtension.bacpac) y guárdelo en el equipo local con el mismo nombre, **SQLDatabaseExtension.bacpac**.

### <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

1. Haga clic en la imagen siguiente para abrir una plantilla de Resource Manager en Azure Portal.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-storage-account-create%2fazuredeploy.json" target="_blank"><img src="./media/resource-manager-tutorial-secure-artifacts/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Especifique las siguientes propiedades:

    * **Suscripción**: Seleccione su suscripción a Azure.
    * **Grupos de recursos**: seleccione **Crear nuevo** y asígnele un nombre. Un grupo de recursos es un contenedor de recursos de Azure para fines de administración. En este tutorial, puede usar el mismo grupo de recursos para la cuenta de almacenamiento y la base de datos SQL de Azure. Anote el nombre de este grupo de recursos; lo necesitará al crear la base de datos SQL de Azure más adelante en los tutoriales.
    * **Ubicación**: Seleccione una región. Por ejemplo, **Centro de EE. UU**. 
    * **Tipo de cuenta de almacenamiento**: use el valor predeterminado, que es **Standard_LRS**.
    * **Ubicación**: use el valor predeterminado, que es **[resourceGroup () .location]**. Es decir, usará la ubicación del grupo de recursos de la cuenta de almacenamiento.
    * **Acepto los términos y condiciones indicados anteriormente**: (seleccionado)
3. Seleccione **Comprar**.
4. Seleccione el icono de notificación (la campana) de la esquina superior derecha del portal para ver el estado de implementación.

    ![Panel de notificaciones del portal del tutorial de Resource Manager](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-portal-notifications-pane.png)
5. Una vez implementada correctamente la cuenta de almacenamiento, seleccione **Ir a Grupos de recursos** en el panel de notificaciones para abrir el grupo de recursos.

### <a name="create-a-blob-container"></a>Creación de contenedor de blobs

Se necesita un contenedor de blobs para cargar archivos. 

1. Seleccione la cuenta de almacenamiento para abrirla. Verá la cuenta de almacenamiento en el grupo de recursos. El nombre de la cuenta de almacenamiento es diferente del que se muestra en la captura de pantalla siguiente.

    ![Cuenta de almacenamiento del tutorial de Resource Manager](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-storage-account.png)

2. Haga clic en el icono de **Blobs**.

    ![Blobs del tutorial de Resource Manager](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-blobs.png)
3. Seleccione **+ Contenedor** en la parte superior para crear un contenedor.
4. Escriba los siguientes valores:

    * **Nombre**: escriba **sqlbacpac**. 
    * **Nivel de acceso público**: use el valor predeterminado, **Privado (sin acceso anónimo)**.
5. Seleccione **Aceptar**.
6. Seleccione **sqlbacpac** para abrir el contenedor recién creado.

### <a name="upload-the-bacpac-file-to-the-container"></a>Carga del archivo BACPAC en el contenedor

1. Seleccione **Cargar**.
2. Escriba los siguientes valores:

    * **Archivos**: siga las instrucciones para seleccionar el archivo BACPAC que descargó anteriormente. El nombre predeterminado es **SQLDatabaseExtension.bacpac**.
    * **Tipo de autenticación**: seleccione **SAS**.  *SAS* es el valor predeterminado.
3. Seleccione **Cargar**.  Una vez que el archivo se carga correctamente, el nombre de archivo debe aparecer en el contenedor.

### <a name="a-namegenerate-a-sas-token-generate-a-sas-token"></a><a name="generate-a-sas-token" />Generación de un token de SAS

1. Haga clic con el botón derecho en **SQLDatabaseExtension.bacpac** en el contenedor y, a continuación, seleccione **Generar SAS**.
2. Escriba los siguientes valores:

    * **Permiso**: use el valor predeterminado, **Lectura**.
    * **Fecha y hora de inicio y caducidad**: El valor predeterminado proporciona ocho horas para usar el token de SAS. Si necesita más tiempo para completar este tutorial, actualice **Expiración**.
    * **Direcciones IP permitidas**: deje este campo en blanco.
    * **Protocolos permitidos**: use el valor predeterminado, **HTTPS**.
    * **Clave de firma**: use el valor predeterminado, **Clave 1**.
3. Seleccione **Generate blob SAS token and URL** (Generar URL y token de SAS del blob).
4. Realizar una copia de la **URL SAS del blob**. En el medio de la URL se encuentra el nombre de archivo **SQLDatabaseExtension.bacpac**.  El nombre de archivo divide la URL divide en tres partes:

   - **Ubicación del artefacto**: https://xxxxxxxxxxxxxx.blob.core.windows.net/sqlbacpac/. Asegúrese de que la ubicación termina con un carácter "/".
   - **Nombre de archivo BACPAC**: SQLDatabaseExtension.bacpac.
   - **Token de SAS de la ubicación del artefacto**: asegúrese de que al token le precede un carácter "?".

     Necesita estos tres valores en [Implementación de la plantilla](#deploy-the-template).

## <a name="open-an-existing-template"></a>Apertura de una plantilla existente

En esta sesión, modificará la plantilla que creó en [Tutorial: Importación de archivos BACPAC de SQL con plantillas de Azure Resource Manager](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md) para llamar al archivo BACPAC con un token de SAS.  La plantilla desarrollada en el tutorial de la extensión de SQL se comparte en [https://armtutorials.blob.core.windows.net/sqlextensionbacpac/azuredeploy.json](https://armtutorials.blob.core.windows.net/sqlextensionbacpac/azuredeploy.json).

1. En Visual Studio Code, seleccione **Archivo**>**Abrir archivo**.
2. En **Nombre de archivo**, pegue el código URL siguiente:

    ```url
    https://armtutorials.blob.core.windows.net/sqlextensionbacpac/azuredeploy.json
    ```
3. Seleccione **Abrir** para abrir el archivo.

    Hay cinco recursos definidos en la plantilla:

   * `Microsoft.Sql/servers`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.sql/2015-05-01-preview/servers).
   * `Microsoft.SQL/servers/securityAlertPolicies`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/securityalertpolicies).
   * `Microsoft.SQL/servers/filewallRules`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.sql/2015-05-01-preview/servers/firewallrules).
   * `Microsoft.SQL/servers/databases`.  Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).
   * `Microsoft.SQL/server/databases/extensions`.  Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions).

     Resulta útil obtener cierta información básica de la plantilla antes de personalizarla.
4. Seleccione **Archivo**>**Guardar como** para guardar una copia del archivo en la máquina local con el nombre **azuredeploy.json**.

## <a name="edit-the-template"></a>Edición de la plantilla

Agregue los siguientes parámetros adicionales:

```json
"_artifactsLocation": {
    "type": "string",
    "metadata": {
        "description": "The base URI where artifacts required by this template are located."
    }
},
"_artifactsLocationSasToken": {
    "type": "securestring",
    "metadata": {
        "description": "The sasToken required to access _artifactsLocation."
    },
    "defaultValue": ""
},
"bacpacFileName": {
    "type": "string",
    "defaultValue": "SQLDatabaseExtension.bacpac",
    "metadata": {
        "description": "The bacpac for configure the database and tables."
    }
}
```

![Parámetros de protección de artefactos del tutorial de Resource Manager](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-secure-artifacts-parameters.png)

Actualice el valor de los dos elementos siguientes:

```json
"storageKey": "[parameters('_artifactsLocationSasToken')]",
"storageUri": "[uri(parameters('_artifactsLocation'), parameters('bacpacFileName'))]",
```

## <a name="deploy-the-template"></a>Implementación de la plantilla

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Consulte la sección [Implementación de la plantilla](./resource-manager-tutorial-create-multiple-instances.md#deploy-the-template) para conocer el procedimiento de implementación. En su lugar, use el siguiente script de implementación de PowerShell.

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the virtual machine admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
$artifactsLocation = Read-Host -Prompt "Enter the artifacts location"
$artifactsLocationSasToken = Read-Host -Prompt "Enter the artifacts location SAS token" -AsSecureString
$bacpacFileName = Read-Host -Prompt "Enter the BACPAC file name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -_artifactsLocation $artifactsLocation `
    -_artifactsLocationSasToken $artifactsLocationSasToken `
    -bacpacFileName $bacpacFileName `
    -TemplateFile "$HOME/azuredeploy.json"
```

Use una contraseña generada. Consulte [Requisitos previos](#prerequisites).
Para los valores de _artifactsLocation, _artifactsLocationSasToken y bacpacFileName, consulte [Generación de un token de SAS](#generate-a-sas-token).

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

En este tutorial, ha implementado una instancia de SQL Server y una instancia de SQL Database y, además, ha importado un archivo BACPAC mediante un token de SAS. Para aprender a implementar los recursos de Azure en varias regiones y a usar prácticas de implementación seguras, consulte

> [!div class="nextstepaction"]
> [Uso de Azure Deployment Manager](./resource-manager-tutorial-deploy-vm-extensions.md)
