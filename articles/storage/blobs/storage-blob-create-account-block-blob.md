---
title: 'Crear una cuenta de almacenamiento blob de bloque: almacenamiento de Azure | Microsoft Docs'
description: Muestra cómo crear una cuenta de almacenamiento de blobs de bloques de Azure con las características de rendimiento premium.
ms.service: storage
ms.topic: conceptual
ms.date: 03/23/2019
author: twooley
ms.author: twooley
ms.subservice: blobs
ms.openlocfilehash: 16ecade7a8d0049e098bb06bd14828b19934f5f5
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58373051"
---
# <a name="create-a-block-blob-storage-account"></a>Crear una cuenta de almacenamiento de blobs de bloque

El tipo de cuenta de almacenamiento de blobs de bloque le permite crear blobs en bloques con características de rendimiento premium. Este tipo de cuenta de almacenamiento está optimizado para cargas de trabajo con las tarifas de transacciones elevadas o que requieren tiempos de acceso muy rápidos. En este artículo se muestra cómo crear una cuenta de almacenamiento de blobs de bloque mediante el portal de Azure, la CLI de Azure o Azure PowerShell.

Para obtener más información acerca de las cuentas de almacenamiento de blobs de bloque, consulte vea [Introducción a la cuenta de almacenamiento de Azure](https://docs.microsoft.com/azure/storage/common/storage-account-overview).

## <a name="create-account-in-the-azure-portal"></a>Crear la cuenta en el portal de Azure

Para crear una cuenta de almacenamiento de blobs de bloque en el portal de Azure, siga estos pasos:

1. En el portal de Azure, seleccione **todos los servicios** > el **almacenamiento** categoría > **cuentas de almacenamiento**.

1. En **cuentas de almacenamiento**, seleccione **agregar**.

1. En el **suscripción** , seleccione la suscripción en la que se va a crear la cuenta de almacenamiento.

1. En el **grupo de recursos** campo, seleccione un grupo de recursos existente o seleccione **crear nuevo**y escriba un nombre para el nuevo grupo de recursos.

1. En el **nombredecuentadealmacenamiento** , escriba un nombre para la cuenta. Tenga en cuenta las siguientes directrices:

   - El nombre debe ser único en Azure.
   - El nombre debe tener entre tres y 24 caracteres.
   - El nombre puede incluir solo números y letras minúsculas.

1. En el **ubicación** campo, seleccione una ubicación para la cuenta de almacenamiento o usar la ubicación predeterminada.

1. Para el resto de la configuración, configure lo siguiente:

   |Campo     |Valor  |
   |---------|---------|
   |**Rendimiento**    |  Seleccione **Premium**.   |
   |**Tipo de cuenta**    | Seleccione **BlockBlobStorage**.      |
   |**Replicación**    |  Deje la configuración predeterminada de **almacenamiento con redundancia local (LRS)**.      |

   ![Muestra el portal de interfaz de usuario para crear una cuenta de almacenamiento de blobs de bloque](media/storage-blob-create-account-block-blob/create-block-blob-storage-account.png)

1. Seleccione **revisar + crear** para revisar la configuración de la cuenta de almacenamiento.

1. Seleccione **Crear**.

### <a name="create-account-using-azure-powershell"></a>Creación de cuenta mediante Azure PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

1. Abra una sesión de Windows PowerShell con privilegios elevados (ejecutar como administrador).

1. Ejecute el siguiente comando para asegurarse de que la versión más reciente de la `Az` está instalado el módulo de PowerShell.

   ```powershell
   Install-Module -Name Az -AllowClobber
   ```

1. Abra una nueva consola de PowerShell e inicie sesión con su cuenta de Azure.

   ```powershell
   Connect-AzAccount -SubscriptionId <SubscriptionID>
   ```

1. Si es necesario, cree un nuevo grupo de recursos. Reemplace los valores entre comillas y ejecute el siguiente comando.

   ```powershell
   $resourcegroup = "new_resource_group_name"
   $location = "region_name"
   New-AzResourceGroup -Name $resourceGroup -Location $location
   ```

1. Cree la cuenta de almacenamiento de blobs de bloque. Reemplace los valores entre comillas y ejecute el siguiente comando.

   ```powershell
   $resourcegroup = "resource_group_name"
   $storageaccount = "new_storage_account_name"
   $location = "region_name"

   New-AzStorageAccount -ResourceGroupName $resourcegroup -Name $storageaccount -Location $location -Kind "BlockBlobStorage" -SkuName "Premium_LRS"
   ```

## <a name="create-account-using-azure-cli"></a>Creación de cuenta mediante la CLI de Azure

Para crear una cuenta de blob de bloque mediante el uso de la CLI de Azure, primero debe instalar CLI de Azure. 2.0.46 o una versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

1. Inicie sesión en la suscripción de Azure.

   ```azurecli
   az login
   ```

1. Si es necesario, cree un nuevo grupo de recursos. Reemplace los valores en corchetes (incluidos los corchetes) y ejecute el siguiente comando.

   ```azurecli
   az group create \
    --name "<new_resource_group_name>" \
    --location "<location>"
   ```

1. Cree la cuenta de almacenamiento de blobs de bloque. Reemplace los valores en corchetes (incluidos los corchetes) y ejecute el siguiente comando.

   ```azurecli
   az storage account create \
    --location "<location>" \
    --name "<new_storage_account_name>" \
    --resource-group "<resource_group_name>" \
    --kind "BlockBlobStorage" \
    --sku "Premium_LRS"
   ```

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre las cuentas de almacenamiento, vea [Introducción a las cuentas de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-account-overview).

- Para más información sobre los grupos de recursos, consulte [Información general de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
