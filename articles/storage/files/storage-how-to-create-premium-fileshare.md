---
title: Crear una cuenta de premium de Azure file storage
description: En este artículo, aprenderá a crear una cuenta de premium de Azure file storage y un recurso compartido de archivos de premium.
services: storage
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 72dec14dde47580313e57bb3b8d7315604929277
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59288432"
---
# <a name="how-to-create-an-azure-premium-file-share"></a>Cómo crear un recurso compartido de archivos de Azure premium

El tipo de cuenta de almacenamiento FileStorage (versión preliminar) representa un nuevo nivel de Azure Files, lo que permite crear recursos compartidos de archivos con las características de rendimiento premium. Estos recursos compartidos de archivos están diseñados para aplicaciones a escala empresarial, que proporciona recursos compartidos de alto rendimiento, IOPS alta y baja latencia coherente y de alto rendimiento.

Este artículo muestra cómo crear este nuevo tipo de cuenta con [portal Azure](https://portal.azure.com/), Azure PowerShell y CLI de Azure.

## <a name="prerequisites"></a>Requisitos previos

Para acceder a Azure Storage, necesitará una suscripción de Azure. Si todavía no tiene una suscripción, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-a-premium-file-share-using-the-azure-portal"></a>Crear un recurso compartido de archivos de premium mediante el portal de Azure

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

### <a name="create-a-filestorage-preview-storage-account"></a>Crear una cuenta de almacenamiento FileStorage (versión preliminar)

Ahora está listo para crear la cuenta de almacenamiento.

Cada cuenta de almacenamiento debe pertenecer a un grupo de recursos de Azure. Un grupo de recursos es un contenedor lógico para agrupar servicios de Azure. Al crear una cuenta de almacenamiento, puede elegir entre crear un grupo de recursos o usar uno existente. En este artículo se muestra cómo crear un nuevo grupo de recursos.

1. En el portal de Azure, seleccione **cuentas de almacenamiento** en el menú izquierdo.

    ![Página principal de Azure portal seleccione la cuenta de almacenamiento](media/storage-how-to-create-premium-fileshare/azure-portal-storage-accounts.png)

1. En la ventana **Cuentas de almacenamiento** que aparece, elija **Agregar**.
1. Seleccione la suscripción en la que se va a crear la cuenta de almacenamiento.
1. En el campo **Grupo de recursos**, haga clic en **Crear nuevo**. Escriba un nombre para el nuevo grupo de recursos, como se muestra en la siguiente imagen.

1. Después, escriba un nombre para la cuenta de almacenamiento. El nombre que elija debe ser único en Azure. El nombre debe tener también una longitud de entre 3 y 24 caracteres y solo puede contener números y letras minúsculas.
1. Seleccione una ubicación para la cuenta de almacenamiento o utilice la ubicación predeterminada.
1. Para **rendimiento** seleccione **Premium**.
1. Seleccione **tipo de cuenta** y elija **FileStorage (versión preliminar)**.
1. Deje **replicación** establecido en su valor predeterminado de **almacenamiento con redundancia local (LRS)**.

    ![Cómo crear una cuenta de almacenamiento de archivos de premium](media/storage-how-to-create-premium-fileshare/premium-files-storage-account.png)

1. Seleccione **Revisar y crear** para revisar la configuración de la cuenta de almacenamiento y crear la cuenta.
1. Seleccione **Crear**.

Una vez creado el recurso de la cuenta de almacenamiento, vaya a él.

### <a name="create-a-premium-file-share"></a>Crear un recurso compartido de archivos de premium

1. En el menú izquierdo de la cuenta de almacenamiento, desplácese a la **del servicio de archivos** sección y, después, seleccione **Files (versión preliminar)**.
1. Seleccione **+ recurso compartido de archivos** para crear un recurso compartido de archivos de premium.
1. Escriba un nombre y una cuota deseada para el recurso compartido de archivo y luego seleccione **crear**.

> [!NOTE]
> Tamaños de recurso compartido aprovisionado está especificado por la cuota del recurso compartido, se facturan los recursos compartidos de archivos del tamaño aprovisionado, consulte el [página de precios](https://azure.microsoft.com/pricing/details/storage/files/) para obtener más detalles.

   ![Crear un recurso compartido de archivos de premium](media/storage-how-to-create-premium-fileshare/create-premium-file-share.png)

### <a name="clean-up-resources"></a>Limpieza de recursos

Si desea limpiar los recursos creados en este artículo, simplemente puede eliminar el grupo de recursos. Eliminando el grupo de recursos, también elimina la cuenta de almacenamiento asociada, así como otros recursos asociados con el grupo de recursos.

## <a name="create-a-premium-file-share-using-powershell"></a>Crear un recurso compartido de archivos de premium con PowerShell

### <a name="create-an-account-using-powershell"></a>Creación de una cuenta con PowerShell

En primer lugar, instale la versión más reciente del módulo [PowerShellGet](https://docs.microsoft.com/powershell/gallery/installing-psget).

A continuación, actualice su módulo de powershell, inicie sesión en su suscripción de Azure, cree un grupo de recursos y, a continuación, cree una cuenta de almacenamiento.

### <a name="upgrade-your-powershell-module"></a>Actualización del módulo de Powershell

Para interactuar con archivos premium con PowerShell, deberá instalar el módulo Az.Storage más reciente.

Para empezar, abra una sesión de PowerShell con permisos elevados.

Instalar el módulo Az.Storage:

```powershell
Install-Module Az.Storage -Repository PSGallery -AllowPrerelease -AllowClobber -Force
```

### <a name="sign-in-to-your-azure-subscription"></a>Inicie sesión en su suscripción de Azure

Use el comando `Login-AzAccount` y siga las instrucciones en pantalla para realizar la autenticación.

```powershell
Login-AzAccount
```

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Para crear un grupo de recursos con PowerShell, use el comando [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup):

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hardcoding it repeatedly
$resourceGroup = "storage-how-to-resource-group"
$location = "westus2"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

### <a name="create-a-filestorage-preview-storage-account"></a>Crear una cuenta de almacenamiento filestorage (versión preliminar)

Para crear una cuenta de almacenamiento FileStorage (versión preliminar) de PowerShell, use el [New AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount) comando:

```powershell
$storageAcct = New-AzStorageAccount -ResourceGroupName $resourceGroup -Name "fileshowto" -SkuName "Premium_LRS" -Location "westus2" -Kind "FileStorage"
```

### <a name="create-a-premium-file-share"></a>Crear un recurso compartido de archivos de premium

Ahora que tiene una cuenta FileStorage, puede crear un recurso compartido de archivos de premium. Use la [New AzStorageShare](/powershell/module/az.storage/New-AzStorageShare) para crear uno.

> [!NOTE]
> Tamaños de recurso compartido aprovisionado está especificado por la cuota del recurso compartido, se facturan los recursos compartidos de archivos del tamaño aprovisionado, consulte el [página de precios](https://azure.microsoft.com/pricing/details/storage/files/) para obtener más detalles.

```powershell
New-AzStorageShare `
   -Name myshare `
   -Context $storageAcct.Context
```

### <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar el grupo de recursos y sus recursos asociados, incluida la nueva cuenta de almacenamiento, use el comando [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup): 

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="create-a-premium-file-share-using-azure-cli"></a>Crear un recurso compartido de archivos de premium mediante la CLI de Azure

Para iniciar Azure Cloud Shell, inicie sesión en el [portal Azure](https://portal.azure.com).

Si quiere iniciar sesión en la instalación local de la CLI, ejecute el comando de inicio de sesión:

```cli
az login
```

### <a name="add-the-cli-extension-for-azure-premium-files"></a>Agregar la extensión de la CLI para los archivos de Azure premium

Para interactuar con archivos premium mediante la CLI, debe agregar una extensión para el shell.

Para ello, escriba el siguiente comando mediante el uso de Cloud Shell o un shell local: `az extension add --name storage-preview`

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Para crear un grupo de recursos con la CLI de Azure, use el comando [az group create](/cli/azure/group).

```azurecli-interactive
az group create `
    --name files-howto-resource-group `
    --location westus2
```

### <a name="create-a-filestorage-preview-storage-account"></a>Crear una cuenta de almacenamiento FileStorage (versión preliminar)

Para crear una cuenta de almacenamiento FileStorage (versión preliminar) desde la CLI de Azure, use el [crear cuenta de almacenamiento de az](/cli/azure/storage/account) comando.

```azurecli-interactive
az storage account create `
    --name fileshowto `
    --resource-group files-howto-resource-group `
    --location westus `
    --sku Premium_LRS `
    --kind FileStorage
```

### <a name="get-the-storage-account-key"></a>Obtención de la clave de la cuenta de almacenamiento

Las claves de cuenta de almacenamiento controlan el acceso a los recursos en una cuenta de almacenamiento, en este artículo, usamos la clave con el fin de crear un recurso compartido de archivos de premium. Las claves se crean automáticamente al crear una cuenta de almacenamiento. Puede obtener las claves de cuenta de almacenamiento con el comando [az storage account keys list](/cli/azure/storage/account/keys):

```azurecli-interactive 
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

### <a name="create-a-premium-file-share"></a>Crear un recurso compartido de archivos de premium

Ahora que tiene una cuenta FileStorage, puede crear un recurso compartido de archivos de premium. Use la [crear recurso compartido de almacenamiento de az](/cli/azure/storage/share) comando para crear uno.

> [!NOTE]
> Tamaños de recurso compartido aprovisionado está especificado por la cuota del recurso compartido, se facturan los recursos compartidos de archivos del tamaño aprovisionado, consulte el [página de precios](https://azure.microsoft.com/pricing/details/storage/files/) para obtener más detalles.

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" 
```

### <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar el grupo de recursos y sus recursos asociados, incluida la nueva cuenta de almacenamiento, use el comando [az group delete](/cli/azure/group).

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha creado una cuenta de almacenamiento de archivos de premium. Para obtener información sobre el rendimiento que ofrece esta cuenta, continúe a la sección de nivel de rendimiento de la Guía de planeación.

> [!div class="nextstepaction"]
> [Niveles de rendimiento de un recurso compartido de archivos](storage-files-planning.md#file-share-performance-tiers)