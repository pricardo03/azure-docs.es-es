---
title: Creación de una cuenta de almacenamiento de Azure Data Lake Storage Gen2 (versión preliminar) | Microsoft Docs
description: Aprenda rápidamente a crear una nueva cuenta de almacenamiento con acceso a Data Lake Storage Gen2 (versión preliminar) con Azure Portal, Azure PowerShell o la CLI de Azure.
services: storage
author: jamesbak
manager: twooley
ms.component: data-lake-storage-gen2
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 06/27/2018
ms.author: jamesbak
ms.openlocfilehash: aafb86e7ebc99ea48e09b34b58682c983fe9f293
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063112"
---
# <a name="quickstart-create-an-azure-data-lake-storage-gen2-preview-storage-account"></a>Guía de inicio rápido: Creación de una cuenta de almacenamiento Azure Data Lake Storage Gen2 (versión preliminar)

Las cuentas de Azure Data Lake Storage Gen2 (versión preliminar) [admiten un servicio de espacio de nombres jerárquico](introduction.md) que proporciona un sistema de archivos basado en el directorio nativo diseñado para trabajar con el sistema de archivos distribuido de Hadoop (HDFS). El acceso a los datos de Data Lake Storage Gen2 desde el sistema de archivos distribuido de Hadoop está disponible en el [controlador ABFS](abfs-driver.md).

Para habilitar las funcionalidades de Data Lake Storage Gen2 en su cuenta de almacenamiento, [rellene la encuesta previa para solicitar acceso](https://aka.ms/adlsgen2signup). Una vez aprobada, podrá crear una nueva cuenta de Data Lake Storage Gen2. En esta guía de inicio rápido se muestra cómo crear una cuenta mediante [Azure Portal](https://portal.azure.com/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) o mediante la [CLI de Azure](https://docs.microsoft.com/cli/azure?view=azure-cli-latest).

> [!NOTE]
> La interfaz de usuario de la creación cuenta en Azure Portal se actualiza una vez que se aprueba la creación de una cuenta de Data Lake Storage Gen2. De la misma manera, los argumentos de PowerShell y CLI relacionados con Data Lake Storage Gen2 solo funcionan una vez que resulta aprobado para la versión preliminar.

## <a name="prerequisites"></a>requisitos previos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

|           | Requisito previo |
|-----------|--------------|
|Portal     | None         |
|PowerShell | Para realizar los pasos de esta guía de inicio rápido, se requiere la versión **5.0.4-preview** del módulo de Azure PowerShell, o cualquier versión posterior. Ejecute `Get-Module -ListAvailable AzureRM` para buscar la versión actual. Si necesita instalarla o actualizarla, consulte [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Instalación y configuración de Azure PowerShell). |
|CLI        | Puede iniciar sesión en Azure y ejecutar comandos de la CLI de Azure de alguna de las dos maneras siguientes: <ul><li>Puede ejecutar los comandos de la CLI desde Azure Portal, en Azure Cloud Shell. </li><li>Puede instalar la CLI y ejecutar los comandos de la CLI localmente.</li></ul>|

Cuando trabaje en la línea de comandos, tiene la opción de ejecutar Azure Cloud Shell o instalar la CLI localmente.

### <a name="use-azure-cloud-shell"></a>Uso de Azure Cloud Shell

Azure Cloud Shell es un shell de Bash gratis que se puede ejecutar directamente en Azure Portal. Tiene la CLI de Azure preinstalada y configurada para utilizar con su cuenta. Haga clic en el botón **Cloud Shell** en el menú de la parte superior derecha de Azure Portal:

[![Cloud Shell](./media/quickstart-create-account/cloud-shell-menu.png)](https://portal.azure.com)

El botón inicia un shell interactivo que puede usar para ejecutar los pasos de esta guía de inicio rápido:

[![Captura de pantalla que muestra la ventana de Cloud Shell en el portal](./media/quickstart-create-account/cloud-shell.png)](https://portal.azure.com)

### <a name="install-the-cli-locally"></a>Instalación local de la CLI

También puede instalar y usar la CLI de Azure localmente. Para realizar este tutorial de inicio rápido, es necesario ejecutar la versión 2.0.4 o superior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0](/cli/azure/install-azure-cli).

## <a name="overview-of-creating-an-azure-data-lake-storage-gen2-account"></a>Introducción a la creación de una cuenta de almacenamiento de Azure Data Lake Gen2

Antes de crear una cuenta, primero debe crear un grupo de recursos que actúe como un contenedor lógico para las cuentas de almacenamiento o cualquier otro recurso de Azure que cree. Si desea limpiar los recursos creados por esta guía de inicio rápido, basta con eliminar el grupo de recursos. Al eliminar el grupo de recursos, también se elimina la cuenta de almacenamiento asociada y otros recursos asociados al grupo. Para más información sobre los grupos de recursos, consulte [Información general de Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

> [!NOTE]
> Debe crear nuevas cuentas de almacenamiento como el tipo **StorageV2 (V2 de propósito general)** para aprovechar las características de Data Lake Storage Gen2.  

Para más información acerca de los tipos de cuentas de almacenamiento, consulte [Opciones de la cuenta de Azure Storage](../common/storage-account-options.md).

Al poner nombre a la cuenta de almacenamiento, tenga en cuenta estas reglas:

- Los nombres de cuentas de almacenamiento deben tener entre 3 y 24 caracteres, y solo pueden contener números y letras minúsculas.
- El nombre de la cuenta de almacenamiento debe ser único dentro de Azure. Dos cuentas de almacenamiento no pueden tener el mismo nombre.

## <a name="create-an-account-using-the-azure-portal"></a>Creación de una cuenta con Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com).

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Para crear un grupo de recursos en Azure Portal, siga estos pasos:

1. En Azure Portal, expanda el menú en el lado izquierdo para abrir el menú de servicios y elija **Grupos de recursos**.
2. Haga clic en el botón **Agregar** para agregar un nuevo grupo de recursos.
3. Escriba un nombre para él.
4. Seleccione la suscripción en la que desea crear el nuevo grupo de recursos.
5. Elija la ubicación del grupo de recursos.
6. Haga clic en el botón **Crear** .  

![Captura de pantalla que muestra la creación del grupo de recursos en Azure Portal](./media/quickstart-create-account/create-resource-group.png)

### <a name="create-a-general-purpose-v2-storage-account"></a>Creación de una cuenta de almacenamiento de uso general v2

Para crear una cuenta de almacenamiento de uso general v2 en Azure Portal, siga estos pasos:

> [!NOTE]
> El espacio de nombres jerárquico solo está habilitado en Oeste de EE. UU. 2 y Centro-oeste de EE. UU. Asegúrese de especificar cualquiera de estas ubicaciones al crear la cuenta de almacenamiento.

1. En Azure Portal, expanda el menú en el lado izquierdo para abrir el menú de servicios y elija **Todos los servicios**. A continuación, desplácese hacia abajo hasta **Almacenamiento** y elija **Cuentas de almacenamiento**. En la ventana **Cuentas de almacenamiento** que aparece, elija **Agregar**.
2. Escriba un nombre para la cuenta de almacenamiento.
3. Deje **Modelo de implementación** establecido en el valor predeterminado.
4. En el campo **Tipo de cuenta**, elija **StorageV2 (uso general v2)**.
5. Establezca **Ubicación** a **Oeste de EE. UU. 2**
6. En el campo **Replicación**, deje la opción **Almacenamiento con redundancia local (LRS)**.
7. Deje estos campos establecidos en sus valores predeterminados: **Replicación**. **Rendimiento**, **Nivel de acceso**.
8. Elija la suscripción en la que desea crear la nueva cuenta de almacenamiento.
9. En la sección **Grupo de recursos**, seleccione **Use existing** (Usar existente) y, a continuación, elija el grupo de recursos que creó en la sección anterior.
10. Mantenga el valor predeterminado de **Redes virtuales**.
11. En la sección **Data Lake Storage Gen2 (versión preliminar)**, establezca **Hierarchical namespace** (Espacio de nombres jerárquico) en **Habilitado**.
12. Haga clic en **Crear** para crear la cuenta de almacenamiento.

![Captura de pantalla que muestra la creación de una cuenta de almacenamiento en Azure Portal](./media/quickstart-create-account/azure-data-lake-storage-account-create.png)

Ahora se crea la cuenta de almacenamiento mediante el portal.

### <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar un grupo de recursos con Azure Portal:

1. En Azure Portal, expanda el menú en el lado izquierdo para abrir el menú de servicios y elija **Grupos de recursos** para mostrar la lista de sus grupos de recursos.
2. Busque el grupo de recursos que desea eliminar y haga clic con el botón derecho en el botón **Más** (**...** ) en el lado derecho de la lista.
3. Seleccione **Eliminar grupo de recursos** y confirme.

## <a name="create-an-account-using-powershell"></a>Creación de una cuenta con PowerShell

Inicie sesión en la suscripción de Azure con el comando `Login-AzureRmAccount` y siga las instrucciones de la pantalla para autenticarse.

```powershell
Login-AzureRmAccount
```

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Para crear un grupo de recursos con PowerShell, use el comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup): 

> [!NOTE]
> El espacio de nombres jerárquico solo está habilitado en Oeste de EE. UU. 2 y Centro-oeste de EE. UU. Asegúrese de especificar cualquiera de estas ubicaciones al crear la cuenta de almacenamiento.

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hardcoding it repeatedly
$resourceGroup = "storage-quickstart-resource-group"
$location = "westus2"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location
```

### <a name="create-a-general-purpose-v2-storage-account"></a>Creación de una cuenta de almacenamiento de uso general v2

Para crear una cuenta de almacenamiento de uso general v2 en PowerShell con almacenamiento con redundancia local (LRS), use el comando [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount):

```powershell
Get-AzureRmLocation | select Location 
$location = "westus2"

New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "storagequickstart" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind StorageV2 
  -HierarchialNamespace $True
```

### <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar el grupo de recursos y sus recursos asociados, incluida la nueva cuenta de almacenamiento, use el comando [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup): 

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="create-an-account-using-azure-cli"></a>Creación de una cuenta con la CLI de Azure 

Para iniciar Azure Cloud Shell, inicie sesión en [Azure Portal](https://portal.azure.com).

Para iniciar sesión en la instalación local de la CLI, ejecute el comando de inicio de sesión:

```cli
az login
```
### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Para crear un grupo de recursos con la CLI de Azure, use el comando [az group create](/cli/azure/group#az_group_create). 

```azurecli-interactive
az group create \
    --name storage-quickstart-resource-group \
    --location westus2
```

> [!NOTE]
> El espacio de nombres jerárquico solo está habilitado en Oeste de EE. UU. 2 y Centro-oeste de EE. UU. Asegúrese de especificar cualquiera de estas ubicaciones al crear la cuenta de almacenamiento.

### <a name="create-a-general-purpose-v2-storage-account"></a>Creación de una cuenta de almacenamiento de uso general v2

Para crear una cuenta de almacenamiento de uso general v2 en la CLI de Azure con almacenamiento con redundancia local, use el comando [az storage account create](/cli/azure/storage/account#az_storage_account_create).

```azurecli-interactive
az storage account create \
    --name storagequickstart \
    --resource-group storage-quickstart-resource-group \
    --location westus2 \
    --sku Standard_LRS \
    --kind StorageV2 \
    --hierarchical-namespace true
```

### <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar el grupo de recursos y sus recursos asociados, incluida la nueva cuenta de almacenamiento, use el comando [az group delete](/cli/azure/group#az_group_delete).

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido ha creado una cuenta de almacenamiento de Data Lake Storage Gen2. Para aprender a cargar y descargar blobs en la cuenta de almacenamiento, siga con la guía de inicio rápido de Blob Storage.

* [Mover datos hacia y desde Azure Blob Storage con AzCopy](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/move-data-to-azure-blob-using-azcopy)