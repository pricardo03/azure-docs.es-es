---
title: 'Creación de una cuenta de almacenamiento: Azure Storage'
description: En este artículo de procedimientos se aprende a crear una cuenta de almacenamiento con Azure Portal, Azure PowerShell o la CLI de Azure. Una cuenta de Azure Storage proporciona un espacio de nombres único en Microsoft Azure para almacenar los objetos de datos que crea y acceder a ellos en Azure Storage.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: article
ms.date: 05/06/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 8375f4c54dc436ecf0694ec5f629c81d3591594d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65234188"
---
# <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

Una cuenta de Azure Storage contiene todos los objetos de datos de Azure Storage: blobs, archivos, colas, tablas y discos. La cuenta de almacenamiento proporciona un espacio de nombres único para los datos de Azure Storage accesibles desde cualquier lugar del mundo a través de HTTP o HTTPS. Los datos de la cuenta de Azure Storage son duraderos y altamente disponibles, seguros y escalables a gran escala.

En este artículo de procedimientos se aprende a crear una cuenta de almacenamiento con [Azure Portal](https://portal.azure.com/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), la [CLI de Azure](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) o una [plantilla de Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Ninguno.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Este artículo de procedimientos requiere la versión 0.7 del módulo Az de Azure PowerShell o cualquier versión posterior. Ejecute `Get-Module -ListAvailable Az` para buscar la versión actual. Si necesita instalarla o actualizarla, consulte el artículo sobre [cómo instalar el módulo de Azure PowerShell](/powershell/azure/install-Az-ps).

# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

Puede iniciar sesión en Azure y ejecutar los comandos de la CLI de Azure de dos maneras:

- Puede ejecutar los comandos de la CLI desde Azure Portal, en Azure Cloud Shell.
- Puede instalar la CLI y ejecutar sus comandos en local.

### <a name="use-azure-cloud-shell"></a>Uso de Azure Cloud Shell

Azure Cloud Shell es un shell de Bash gratuito que puede ejecutarse directamente en Azure Portal. La CLI de Azure está preinstalada y configurada para su uso con la cuenta. Haga clic en el botón **Cloud Shell** del menú situado en la sección superior derecha de Azure Portal:

[![Cloud Shell](./media/storage-quickstart-create-account/cloud-shell-menu.png)](https://portal.azure.com)

El botón inicia un shell interactivo que se puede usar para ejecutar los pasos de este artículo de procedimientos:

[![Captura de pantalla en la que aparece la ventana de Cloud Shell del portal](./media/storage-quickstart-create-account/cloud-shell.png)](https://portal.azure.com)

### <a name="install-the-cli-locally"></a>Instalación local de la CLI

También puede instalar y usar la CLI de Azure localmente. Para este artículo de procedimientos es preciso usar la versión 2.0.4 de la CLI de Azure o cualquier versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

# <a name="templatetabtemplate"></a>[Plantilla](#tab/template)

Ninguno.

---

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Inicie sesión en el [Azure Portal](https://portal.azure.com).

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Inicie sesión en la suscripción de Azure con el comando `Connect-AzAccount` y siga las instrucciones que aparecen en pantalla para autenticarse.

```powershell
Connect-AzAccount
```

# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para iniciar Azure Cloud Shell, inicie sesión en [Azure Portal](https://portal.azure.com).

Para iniciar sesión en la instalación local de la CLI, ejecute el comando [az login](/cli/azure/reference-index#az-login):

```cli
az login
```

# <a name="templatetabtemplate"></a>[Plantilla](#tab/template)

N/D

---

## <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

Ahora ya está listo para crear la cuenta de almacenamiento.

Cada cuenta de almacenamiento debe pertenecer a un grupo de recursos de Azure. Un grupo de recursos es un contenedor lógico para agrupar servicios de Azure. Al crear una cuenta de almacenamiento, puede elegir entre crear un grupo de recursos o usar uno existente. En este artículo se muestra cómo crear un nuevo grupo de recursos.

Una cuenta de almacenamiento de **uso general v2** proporciona acceso a todos los servicios de Azure Storage: Blob, File, Queue, Table y Disk. Los pasos indicados aquí crean una cuenta de almacenamiento de uso general v2, pero los pasos para crear cualquier otro tipo de cuenta de almacenamiento son similares.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

En primer lugar, cree un grupo de recursos con PowerShell mediante el comando [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup):

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hard-coding it repeatedly
$resourceGroup = "storage-resource-group"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

Si no está seguro de qué región especificar para el parámetro `-Location`, puede recuperar una lista de regiones admitidas para la suscripción con el comando [Get-AzLocation](/powershell/module/az.resources/get-azlocation):

```powershell
Get-AzLocation | select Location
$location = "westus"
```

Luego, para crear una cuenta de almacenamiento de uso general v2 con almacenamiento con redundancia local con acceso de lectura (RA-GRS), use el comando [New-AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount). Recuerde que el nombre de la cuenta de almacenamiento debe ser único en Azure, así que reemplace el valor de marcador de posición entre corchetes por su propio valor único:

```powershell
New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name <account-name> `
  -Location $location `
  -SkuName Standard_RAGRS `
  -Kind StorageV2 
```

Para crear una cuenta de almacenamiento de uso general v2 con otra opción de replicación, sustituya el valor deseado de la tabla siguiente por el parámetro **SkuName**.

|Opción Replicación  |Parámetro SkuName  |
|---------|---------|
|Almacenamiento con redundancia local (LRS)     |Standard_LRS         |
|Almacenamiento con redundancia de zona (ZRS)     |Standard_ZRS         |
|Almacenamiento con redundancia geográfica (GRS)     |Standard_GRS         |
|Almacenamiento con redundancia geográfica con acceso de lectura (GRS)     |Standard_RAGRS         |

# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

Primero, cree un grupo de recursos con la CLI de Azure mediante el comando [az group create](/cli/azure/group#az_group_create).

```azurecli-interactive
az group create \
    --name storage-resource-group \
    --location westus
```

Si no está seguro de qué región especificar para el parámetro `--location`, puede recuperar una lista de regiones admitidas para la suscripción con el comando [az account list-locations](/cli/azure/account#az_account_list).

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

Luego, para crear una cuenta de almacenamiento de uso general v2 con almacenamiento con redundancia local con acceso de lectura (RA-GRS), use el comando [az storage account create](/cli/azure/storage/account#az_storage_account_create). Recuerde que el nombre de la cuenta de almacenamiento debe ser único en Azure, así que reemplace el valor de marcador de posición entre corchetes por su propio valor único:

```azurecli-interactive
az storage account create \
    --name <account-name> \
    --resource-group storage-resource-group \
    --location westus \
    --sku Standard_RAGRS \
    --kind StorageV2
```

Para crear una cuenta de almacenamiento de uso general v2 con otra opción de replicación, sustituya el valor deseado de la tabla siguiente por el parámetro **sku**.

|Opción Replicación  |Parámetro sku  |
|---------|---------|
|Almacenamiento con redundancia local (LRS)     |Standard_LRS         |
|Almacenamiento con redundancia de zona (ZRS)     |Standard_ZRS         |
|Almacenamiento con redundancia geográfica (GRS)     |Standard_GRS         |
|Almacenamiento con redundancia geográfica con acceso de lectura (GRS)     |Standard_RAGRS         |

# <a name="templatetabtemplate"></a>[Plantilla](#tab/template)

Puede usar Azure PowerShell o la CLI de Azure para implementar una plantilla de Resource Manager y crear así una cuenta de almacenamiento. La plantilla empleada en este artículo de procedimientos proviene de [Plantillas de inicio rápido de Azure Resource Manager](https://azure.microsoft.com/resources/templates/101-storage-account-create/). Para ejecutar los scripts, seleccione **Pruébelo** para abrir Azure Cloud Shell. Para pegar el script, haga clic con el botón derecho en el shell y, a continuación, seleccione **Pegar**.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
az group create --name $resourceGroupName --location "$location" &&
az group deployment create --resource-group $resourceGroupName --template-file "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

Para aprender a crear plantillas, consulte:

- [Documentación de Azure Resource Manager](/azure/azure-resource-manager/).
- [Referencia de plantilla de cuenta de almacenamiento](/azure/templates/microsoft.storage/allversions).
- [Ejemplos de plantillas de cuenta de almacenamiento adicionales](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Storage).

---

Para más información acerca de las opciones de replicación, consulte [Opciones de replicación de Azure Storage](storage-redundancy.md).

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar los recursos creados mediante este artículo de procedimientos, puede eliminar el grupo de recursos. Al eliminar el grupo de recursos, también se elimina la cuenta de almacenamiento asociada y cualquier otro recurso que esté asociado a dicho grupo.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Para quitar un grupo de recursos desde Azure Portal:

1. En Azure Portal, expanda el menú de la izquierda para abrir el menú de servicios y elija **Grupos de recursos** para ver una lista con sus grupos de recursos.
2. Busque el grupo de recursos que desea eliminar y haga clic con el botón derecho en el botón **Más** ( **...** ) situado en la parte derecha de la lista.
3. Seleccione **Eliminar grupo de recursos** y confirme.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Para quitar el grupo de recursos y sus recursos asociados, incluida la nueva cuenta de almacenamiento, use el comando [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup):

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para quitar el grupo de recursos y sus recursos asociados, incluida la nueva cuenta de almacenamiento, use el comando [az group delete](/cli/azure/group#az_group_delete).

```azurecli-interactive
az group delete --name storage-resource-group
```

# <a name="templatetabtemplate"></a>[Plantilla](#tab/template)

Para quitar el grupo de recursos y sus recursos asociados, incluida la nueva cuenta de almacenamiento, use Azure PowerShell o la CLI de Azure.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
```

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

---

## <a name="next-steps"></a>Pasos siguientes

En este artículo de procedimientos se ha creado una cuenta de almacenamiento estándar de uso general v2. Para aprender a cargar y descargar blobs en la cuenta de almacenamiento, vaya a alguna de las guías de inicio rápido de Blob Storage.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

> [!div class="nextstepaction"]
> [Uso de blobs con Azure Portal](../blobs/storage-quickstart-blobs-portal.md)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

> [!div class="nextstepaction"]
> [Uso de blobs con PowerShell](../blobs/storage-quickstart-blobs-powershell.md)

# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

> [!div class="nextstepaction"]
> [Uso de blobs con la CLI de Azure](../blobs/storage-quickstart-blobs-cli.md)

# <a name="templatetabtemplate"></a>[Plantilla](#tab/template)

> [!div class="nextstepaction"]
> [Uso de blobs con Azure Portal](../blobs/storage-quickstart-blobs-portal.md)

---
